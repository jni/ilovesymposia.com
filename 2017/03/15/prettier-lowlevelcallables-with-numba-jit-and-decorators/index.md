<!--
.. title: Prettier LowLevelCallables with Numba JIT and decorators
.. slug: prettier-lowlevelcallables-with-numba-jit-and-decorators
.. date: 2017-03-15 00:14:49
.. tags: Numba,open-source,Planet SciPy,programming,Python,SciPy
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>In my <a href="https://ilovesymposia.com/2017/03/12/scipys-new-lowlevelcallable-is-a-game-changer/">recent post</a>, I extolled the virtues of SciPy 0.19's <code>LowLevelCallable</code>. I did lament, however, that for <code>generic_filter</code>, the <code>LowLevelCallable</code> interface is a good deal uglier than the standard function interface. In the latter, you merely need to provide a function that takes the values within a pixel neighbourhood, and outputs a single value — an arbitrary function of the input values. That is a Wholesome and Good filter function, the way God intended.

In contrast, a LowLevelCallable takes the following signature:

```python

&gt;&gt;&gt; from llc import jit_filter_function

```

The source code is <a href="https://github.com/jni/llc-tools">on GitHub</a>. Currently it only covers <code>ndi.generic_filter</code>'s signature, and only with Numba, but I hope to gradually expand it to cover all the functions that take <code>LowLevelCallables</code> in SciPy, as well as support Cython. Pull requests are welcome!</p></body></html>
