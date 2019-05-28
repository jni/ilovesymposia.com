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

```c
int callback(double *buffer, intptr_t filter_size, 
             double *return_value, void *user_data)
```

That’s not very Pythonic at all. In fact, it’s positively Conic (TM). For those that don’t know,
[pointers are evil](https://www.quora.com/Why-do-people-think-pointers-are-evil),
so let’s aim to avoid their use.

<!-- TEASER_END -->

“But Juan!”, you are no doubt exclaiming. “Juan! Didn’t you *just* tell us how to use pointers in Numba cfuncs, and tell us how great it was because it was so fast?”

Indeed I did. But it left a bad taste in my mouth. Although I felt that the tradeoff was worth it for such a phenomenal speed boost (300x!), I was unsatisfied. So I started immediately to look for a tidier solution. One that would let me write proper filter functions while still taking advantage of LowLevelCallables.

It turns out Numba cfuncs *can call Numba jitted functions*, so, with a little bit of decorator magic, it’s now ludicrously easy to write performant callables for SciPy using just pure Python. (If you don’t know what Numba JIT is, [read my earlier post](https://ilovesymposia.com/2016/12/20/numba-in-the-real-world/).) As in the last post, let’s look at `grey_erosion` as a baseline benchmark:

```python
>>> import numpy as np
>>> footprint = np.array([[0, 1, 0],
...                       [1, 1, 1],
...                       [0, 1, 0]], dtype=bool)
>>> from scipy import ndimage as ndi
>>>
>>> %timeit ndi.grey_erosion(image, footprint=fp)
1 loop, best of 3: 160 ms per loop
```

Now, we write a decorator that uses Numba jit *and* Numba cfunc to make a LowLevelCallable suitable for passing directly into `generic_filter`:

```python
import numba
from numba import cfunc, carray
from numba.types import intc, CPointer, float64, intp, voidptr
from scipy import LowLevelCallable

def jit_filter_function(filter_function):
    jitted_function = numba.jit(filter_function, nopython=True)
    @cfunc(intc(CPointer(float64), intp, CPointer(float64), voidptr))
    def wrapped(values_ptr, len_values, result, data):
        values = carray(values_ptr, (len_values,), dtype=float64)
        result[0] = jitted_function(values)
        return 1
    return LowLevelCallable(wrapped.ctypes)
```

If you haven’t seen decorators before, read [this primer](https://realpython.com/blog/python/primer-on-python-decorators/) from Real Python. To summarise, we’ve written a function that takes as input a Python function, and outputs a LowLevelCallable. Here’s how to use it:

```python
@jit_filter_function
def fmin(values):
    result = np.inf
    for v in values:
        if v < result:
            result = v
    return result
```

As you can see, the `fmin` function definition looks just like a normal Python function. All the magic happens when we attach our `@jit_filter_function` decorator to the top of the function. Let’s see it in action:


```python
>>> %timeit ndi.generic_filter(image, fmin, footprint=fp)
10 loops, best of 3: 92.9 ms per loop
```

Wow! `numba.jit` is actually over 70% faster than `grey_erosion` or the plain `cfunc` approach!

In case you want to use this, I’ve made a package [available on PyPI](https://pypi.python.org/pypi/llc), so you can actually pip install it right now with `pip install llc` (for low-level callable), and then:

```python
from llc import jit_filter_function
```

The source code is <a href="https://github.com/jni/llc-tools">on GitHub</a>. Currently it only covers <code>ndi.generic_filter</code>'s signature, and only with Numba, but I hope to gradually expand it to cover all the functions that take <code>LowLevelCallables</code> in SciPy, as well as support Cython. Pull requests are welcome!</p></body></html>
