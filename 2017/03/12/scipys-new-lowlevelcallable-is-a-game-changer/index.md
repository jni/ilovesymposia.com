<!--
.. title: SciPy's new LowLevelCallable is a game-changer
.. slug: scipys-new-lowlevelcallable-is-a-game-changer
.. date: 2017-03-12 14:41:41
.. tags: Numba,open-source,Planet SciPy,programming,Python,science,SciPy
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>... and combines rather well with that other game-changing library I like, <a href="https://ilovesymposia.com/2016/12/20/numba-in-the-real-world/">Numba</a>.

I've <a href="https://ilovesymposia.com/2015/12/10/the-cost-of-a-python-function-call/">lamented before</a> that function calls are expensive in Python, and that this severely hampers many functions that <em>should</em> be insanely useful, such as SciPy's <a href="https://docs.scipy.org/doc/scipy/reference/generated/scipy.ndimage.generic_filter.html#scipy.ndimage.generic_filter"><code>ndimage.generic_filter</code></a>.

To illustrate this, let's look at image <em>erosion</em>, which is the replacement of each pixel in an image by the minimum of its neighbourhood. <code>ndimage</code> has a fast C implementation, which serves as a perfect benchmark against the generic version, using a generic filter with <code>min</code> as the operator. Let's start with a 2048 x 2048 random image:

```http://cython.readthedocs.io/en/latest/src/userguide/language_basics.html#types

&gt;&gt;&gt; %timeit ndi.generic_filter(image, LowLevelCallable(nbmin.ctypes), footprint=footprint)
10 loops, best of 3: 113 ms per loop

```

That's right: it's even marginally <em>faster</em> than the pure C version! I almost cried when I ran that.

</p><hr>

Higher-order functions, ie functions that take other functions as input, enable powerful, concise, elegant <a href="https://ilovesymposia.com/2014/06/24/a-clever-use-of-scipys-ndimage-generic_filter-for-n-dimensional-image-processing/">expressions</a> of various algorithms. Unfortunately, these have been hampered in Python for large-scale data processing because of Python's function call overhead. SciPy's latest update goes a long way towards redressing this.</body></html>
