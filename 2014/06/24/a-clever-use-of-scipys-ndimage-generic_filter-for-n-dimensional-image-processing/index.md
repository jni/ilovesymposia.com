<!--
.. title: A clever use of SciPy's ndimage.generic_filter for n-dimensional image processing
.. slug: a-clever-use-of-scipys-ndimage-generic_filter-for-n-dimensional-image-processing
.. date: 2014-06-24 01:48:07
.. tags: Planet SciPy,structuring element,programming,Research Blogging
.. category: 
.. link: 
.. description: 
.. type: text
.. status: published
-->

<html><body><p>This year I am privileged to be a mentor in the <a href="https://www.google-melange.com/gsoc/homepage/google/gsoc2014">Google Summer of Code</a> for the <a href="http://scikit-image.org">scikit-image</a> project, as part of the Python Software Foundation organisation. Our student, <a href="https://github.com/vighneshbirodkar">Vighnesh Birodkar</a>, recently came up with a clever use of SciPy’s <a href="http://docs.scipy.org/doc/scipy/reference/generated/scipy.ndimage.filters.generic_filter.html"><code>ndimage.generic_filter</code></a> that is certainly worth sharing widely.

Vighnesh is <a href="http://www.google-melange.com/gsoc/proposal/public/google/gsoc2014/vighneshbirodkar/5870670537818112">tasked</a> with implementing region adjacency graphs and graph based methods for image segmentation. He initially wrote specific functions for 2D and 3D images, and I suggested that he should merge them: either with n-dimensional code, or, at the very least, by making 2D a special case of 3D. He chose the former, and produced extremely elegant code. Three nested for loops and a large number of neighbour computations were replaced by a function call and a simple loop. Read on to find out how.

<!-- TEASER_END -->

Iterating over an array of unknown dimension is not trivial a priori, but thankfully, someone else has already solved that problem: NumPy’s <a href="http://docs.scipy.org/doc/numpy/reference/generated/numpy.nditer.html"><code>nditer</code></a> and <a href="http://docs.scipy.org/doc/numpy/reference/generated/numpy.ndindex.html"><code>ndindex</code></a> functions allow one to efficiently iterate through every point of an n-dimensional array. However, that still leaves the problem of finding neighbors, to determine which regions are adjacent to each other. Again, this is not trivial to do in nD.

scipy.ndimage provides a suitable function, <code>generic_filter</code>. Typically, a filter is used to iterate a “selector” (called a structuring element) over an array, compute some function of all the values covered by the structuring element, and replace the central value by the output of the function. For example, using the structuring element:

```python
fp = np.array([[0, 1, 0],
               [1, 1, 1],
               [0, 1, 0]], np.uint8)
```

and the function <code>np.median</code> on a 2D image produces a median filter over a pixel’s immediate neighbors. That is,

```python
import functools
median_filter = functools.partial(generic_filter,
                                  function=np.median,
                                  footprint=fp)
```

Here, we don’t want to create an output array, but an output graph. What to do? As it turns out, Python’s pass-by-reference allowed Vighnesh to do this quite easily using the “extra_arguments” keyword to <code>generic_filter</code>: we can write a filter function that receives the graph and updates it when two distinct values are adjacent! <code>generic_filter</code> passes all values covered by a structuring element as a flat array, in the array order of the structuring element. So Vighnesh wrote the following function:

```python
def _add_edge_filter(values, g):
    """Add an edge between first element in `values` and
    all other elements of `values` in the graph `g`.
    `values[0]` is expected to be the central value of
    the footprint used.

    Parameters
    ----------
    values : array
        The array to process.
    g : RAG
        The graph to add edges in.

    Returns
    -------
    0.0 : float
        Always returns 0.

    """
    values = values.astype(int)
    current = values[0]
    for value in values[1:]:
        g.add_edge(current, value)
    return 0.0
```

Then, using the footprint:

```python
fp = np.array([[0, 0, 0],
               [0, 1, 1],
               [0, 1, 0]], np.uint8)
```

(or its n-dimensional analog), this filter is called as follows on <code>labels</code>, the image containing the region labels:

```python
filters.generic_filter(labels,
                       function=_add_edge_filter,
                       footprint=fp,
                       mode='nearest',
                       extra_arguments=(g,))
```

This is a rather unconventional use of generic_filter, which is normally used for its output array. Note how the return value of the filter function, <code>_add_edge_filter</code>, is just 0! In our case, the output array contains all 0s, but we use the filter <em>exclusively for its side-effect</em>: adding an edge to the graph g when there is more than one unique value in the footprint. That’s cool.

Continuing, in this first RAG implementation, Vighnesh wanted to segment according to average color, so he further needed to iterate over each pixel/voxel/hypervoxel and keep a running total of the color and the pixel count. He used elements in the graph node dictionary for this and updated them using <code>ndindex</code>:

```python
for index in np.ndindex(labels.shape):
    current = labels[index]
    g.node[current]['pixel count'] += 1
    g.node[current]['total color'] += image[index]
```

Thus, together, numpy’s <code>nditer</code>, <code>ndindex</code>, and scipy.ndimage’s <code>generic_filter</code> provide a powerful way to perform a large variety of operations on n-dimensional arrays… Much larger than I’d realised!

You can see Vighnesh’s complete pull request <a href="https://github.com/scikit-image/scikit-image/pull/1031">here</a> and follow his blog <a href="http://vcansimplify.wordpress.com/">here</a>.

If you use NumPy arrays and their massive bag of tricks, please cite the paper below!

<span class="Z3988" title="ctx_ver=Z39.88-2004&amp;rft_val_fmt=info%3Aofi%2Ffmt%3Akev%3Amtx%3Ajournal&amp;rft.jtitle=Computing+in+Science+and+Engineering+13%2C+2+%282011%29+22-30&amp;rft_id=info%3Aarxiv%2F1102.1523v1&amp;rfr_id=info%3Asid%2Fresearchblogging.org&amp;rft.atitle=The+NumPy+array%3A+a+structure+for+efficient+numerical+computation&amp;rft.issn=&amp;rft.date=2011&amp;rft.volume=&amp;rft.issue=&amp;rft.spage=&amp;rft.epage=&amp;rft.artnum=&amp;rft.au=Stefan+Van+Der+Walt&amp;rft.au=S.+Chris+Colbert&amp;rft.au=Ga%C3%ABl+Varoquaux&amp;rfe_dat=bpr3.included=1;bpr3.tags=Computer+Science+%2F+Engineering%2CSoftware+Engineering">Stefan Van Der Walt, S. Chris Colbert, &amp; Gaël Varoquaux (2011). The NumPy array: a structure for efficient numerical computation <span style="font-style:italic;">Computing in Science and Engineering 13, 2 (2011) 22-30</span> arXiv: <a rev="review" href="http://arxiv.org/abs/1102.1523v1">1102.1523v1</a></span></p></body></html>
