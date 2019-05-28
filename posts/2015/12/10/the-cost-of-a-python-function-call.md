<!--
.. title: The cost of a Python function call
.. slug: the-cost-of-a-python-function-call
.. date: 2015-12-10 01:04:00
.. tags: Planet SciPy,programming,Python
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>I've read in various places that the Python function call overhead is very high. As I was parroting this "fact" to <a href="https://twitter.com/edschofield">Ed Schofield</a> recently, he asked me what the cost of a function actually was. I had no idea. This prompted us to do a few quick benchmarks.

The short version is that it takes about 150ns to call a function in Python (on my laptop). This doesn't sound like a lot, but it means that you can make <em>at most</em> 6.7 million calls per second, two to three orders of magnitude slower than your processor's clock speed.

If you want your function to do something, such as, oh, I don't know, <em>receive an input argument</em>, this goes up to 350ns, throttling you at 2.8 million calls per second.

</p><h2>Benchmarking function calls</h2>

I cleaned up Ed's and my initial experiments to make a small <a href="https://github.com/jni/performance-tests/blob/master/function-calls/f.py">module</a> and <a href="https://github.com/jni/performance-tests/blob/master/function-calls/timer.py">timer</a> to measure all these values. You can clone the <a href="https://github.com/jni/performance-tests">repo</a> and run <code>python function-calls/timer.py</code> to check the numbers on your machine.

The benchmarks are variations of comparing the execution time of:

```python
for i in range(n):
    pass
```

and:

```python
def f():
    pass

for i in range(n):
    f()
```

for some suitably large <code>n</code>. As I mentioned above, that comes out to an absolute minimum of 150ns per function call.

<h2>What this means</h2>

I've been making a fuss over the past year about the excellent Toolz and the way it enables elegant streaming data processing. (See my <a href="https://github.com/jni/streaming-talk">demo repo</a> and my <a href="https://www.youtube.com/watch?v=cMo4fnCbSPc">EuroSciPy talk</a>.) You can read data from a modern SSD at speeds approaching 500MB/s. If you want to stream each byte through Python functions, you'll <em>instantly</em> lose two orders of magnitude of speed. And, the more functions you use, the slower you'll go, which discourages functional programming and modularity — the very things I was trying to promote!

In the DNA sequence processing I demo in the talk, I get a throughput of about 0.5MB/s. On one hand, this is kind of OK because we are using effectively zero RAM, so we can just let the code run over lunch. On the other, it's starting to bug me that 99% of my processor time is spent on Python function calls, rather than on actual data crunching.

This is a problem for Python. To work on seriously big data, you need to drop into a library written in C, such as NumPy or Pandas. You need to do this on a high level: any per-byte or per-data-element processing <em>cannot</em> be in Python, if you don't want to waste your processor's cycles. Python's ecosystem is Insanely Great, so this is mostly fine, but it does limit your ability to research or implement cool new methods using Python.

As an example, the <a href="http://docs.scipy.org/doc/scipy/reference/generated/scipy.ndimage.filters.generic_filter.html"><code>generic_filter</code></a> function in SciPy's <code>ndimage</code> package has infinitely many cool <a href="http://ilovesymposia.com/2014/06/24/a-clever-use-of-scipys-ndimage-generic_filter-for-n-dimensional-image-processing/">uses</a>, but using it to process a 100MB image (which is small in biology) would take 15 seconds in function call overhead <em>alone</em>. Lest you think this is reasonable, SciPy's greyscale erosion, implemented in C, takes less than 4 seconds on an image that size. A lot of my once-lackadaisical attitude towards Python performance stemmed from not knowing how long things <em>should</em> take. A lot less than they do, it turns out.

<h2>What to do about it</h2>

As I mentioned, Python's high performance libraries are many and great. Look hard for optimised libraries that already do what you want. Try to express what you want to do as combinations of functions from NumPy, SciPy, Pandas, scikit-image, scikit-learn, and so on. Minimise the amount of time spent in Python. This is advice that you learn early on in scientific Python programming, but I didn't appreciate just how important it is.

At some point, that approach will fail, and you will want to do something cute and custom with your data points. Reach for <a href="http://cython.readthedocs.org">Cython</a> sooner rather than later. As a primer, I recommend Stefan Behnel's excellent <a href="https://youtu.be/GmxZfZjEjZo">tutorial</a> from EuroSciPy 2015.

There is also Continuum's <a href="http://numba.pydata.org">Numba</a>, which is sometimes easier to use than Cython. I don't have any experience with it so I can't comment much here. However, I'd consider it a very valuable project to implement <code>generic_filter</code> in Numba.
In the long-run, these are all workarounds, and I hope that the Python interpreter itself becomes faster, though there are few signs of that happening.

If you have other ideas on how to get around Python's function call cost, please let me know in the comments!</body></html>
