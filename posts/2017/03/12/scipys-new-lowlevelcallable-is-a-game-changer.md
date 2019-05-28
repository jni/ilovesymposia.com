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

<!-- TEASER_END -->

To illustrate this, let's look at image <em>erosion</em>, which is the replacement of each pixel in an image by the minimum of its neighbourhood. <code>ndimage</code> has a fast C implementation, which serves as a perfect benchmark against the generic version, using a generic filter with <code>min</code> as the operator. Let's start with a 2048 x 2048 random image:

```python
>>> import numpy as np
>>> image = np.random.random((2048, 2048))
```

and a neighbourhood “footprint” that picks out the pixels to the left and right, and above and below, the centre pixel:

```python
>>> footprint = np.array([[0, 1, 0],
...                       [1, 1, 1],
...                       [0, 1, 0]], dtype=bool)
```

Now, we measure the speed of `grey_erosion` and `generic_filter`. Spoiler alert: it’s not pretty.

```python
>>> from scipy import ndimage as ndi
>>> %timeit ndi.grey_erosion(image, footprint=footprint)
10 loops, best of 3: 118 ms per loop
>>> %timeit ndi.generic_filter(image, np.min, footprint=footprint)
1 loop, best of 3: 27 s per loop
```

As you can see, with Python functions, `generic_filter` is unusable for anything but the tiniest of images.

A few months ago, I was
[trying](https://groups.google.com/a/continuum.io/d/msg/numba-users/HMg_65R8KZE/RnysYokGAwAJ)
to get around this by using Numba-compiled functions, but the way to feed C
functions to SciPy was different depending on which part of the library you
were using. `scipy.integrate` used ctypes, while `scipy.ndimage` used `PyCObjects` or
`PyCapsules`, depending on your Python version, and Numba only supported the
former method at the time. (Plus, this topic starts to stretch my understanding
of low-level Python, so I felt there wasn’t much I could do about it.)

Enter [this pull request](https://github.com/scipy/scipy/pull/6509) to SciPy
from Pauli Virtanen, which is live in the most recent SciPy version, 0.19. It
unifies all C-function interfaces within SciPy, and Numba already supports this
format. It takes a bit of gymnastics, but it works! It really works!

(By the way, the release is full of little gold nuggets. If you use SciPy at
all, the release notes are well worth a read.)

First, we need to define a C function of the appropriate signature. Now, you
might think this is the same as the Python signature, taking in an array of
values and returning a single value, but that would be too easy! Instead, we
have to go back to some C-style programming with pointers and array sizes. From
the `generic_filter` documentation:

> This function also accepts low-level callback functions with one of the
> following signatures and wrapped in scipy.LowLevelCallable:
> 
> ```c
> int callback(double *buffer, npy_intp filter_size, 
>              double *return_value, void *user_data)
> int callback(double *buffer, intptr_t filter_size, 
>              double *return_value, void *user_data)
> ```
> 
> The calling function iterates over the elements of the input and output
> arrays, calling the callback function at each element. The elements within
> the footprint of the filter at the current element are passed through the
> buffer parameter, and the number of elements within the footprint through
> filter_size. The calculated value is returned in return_value. user_data is
> the data pointer provided to scipy.LowLevelCallable as-is.
> 
> The callback function must return an integer error status that is zero if
> something went wrong and one otherwise. 

(Let’s leave aside that crazy reversal of Unix convention of the past 50 years
in the last paragraph, except to note that our function must return 1 or it
will be killed.)

So, we need a Numba cfunc that takes in:

 *   a double pointer pointing to the values within the footprint,
 *   a pointer-sized integer that specifies the number of values in the footprint,
 *   a double pointer for the result, and
 *   a void pointer, which could point to additional parameters, but which we can ignore for now.

The Numba type names are listed in [this page](http://numba.pydata.org/numba-doc/dev/reference/types.html#numba-types). Unfortunately, at the time of
writing, there’s no mention of how to make pointers there, but finding such a
reference [was not too hard](http://numba.pydata.org/numba-doc/dev/user/cfunc.html#signature-specification). (Incidentally, it would make a good contribution to
Numba’s documentation to add CPointer to the Numba types page.)

So, armed with all that documentation, and after much trial and error, I was
finally ready to write that C callable:

```python
>>> from numba import cfunc, carray
>>> from numba.types import intc, intp, float64, voidptr
>>> from numba.types import CPointer
>>> 
>>> 
>>> @cfunc(intc(CPointer(float64), intp,
...             CPointer(float64), voidptr))
... def nbmin(values_ptr, len_values, result, data):
...     values = carray(values_ptr, (len_values,), dtype=float64)
...     result[0] = np.inf
...     for v in values:
...         if v < result[0]:
...             result[0] = v
...     return 1
```

The only other tricky bits I had to watch out for while writing that function were as follows:


 * remembering that there’s two ways to de-reference a pointer in C: `*ptr`,
   which is not valid Python and thus not valid Numba, and `ptr[0]`. So, to place
   the result at the given double pointer, we use the latter syntax. (If you
   prefer to use Cython, the same rule applies.)
 * Creating an array out of the `values_ptr` and `len_values` variables, as shown
   here. That’s what enables the `for v in values` Python-style access to the
   array.

Ok, so now what you’ve been waiting for. How did we do? First, to recap, the original benchmarks:

```python
>>> from scipy import ndimage as ndi
>>> %timeit ndi.grey_erosion(image, footprint=footprint)
10 loops, best of 3: 118 ms per loop
>>> %timeit ndi.generic_filter(image, np.min, footprint=footprint)
1 loop, best of 3: 27 s per loop
```

And now, with our new Numba cfunc:

```python
>>> %timeit ndi.generic_filter(image, LowLevelCallable(nbmin.ctypes), footprint=footprint)
10 loops, best of 3: 113 ms per loop
```


That's right: it's even marginally <em>faster</em> than the pure C version! I almost cried when I ran that.

</p>

<hr>

Higher-order functions, ie functions that take other functions as input, enable powerful, concise, elegant <a href="https://ilovesymposia.com/2014/06/24/a-clever-use-of-scipys-ndimage-generic_filter-for-n-dimensional-image-processing/">expressions</a> of various algorithms. Unfortunately, these have been hampered in Python for large-scale data processing because of Python's function call overhead. SciPy's latest update goes a long way towards redressing this.</body></html>
