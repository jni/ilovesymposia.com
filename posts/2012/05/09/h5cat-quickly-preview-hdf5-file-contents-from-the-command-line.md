<!--
.. title: h5cat: quickly preview HDF5 file contents from the command-line
.. slug: h5cat-quickly-preview-hdf5-file-contents-from-the-command-line
.. date: 2012-05-09 16:29:05
.. tags: Planet SciPy,programming,software
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>As a first attempt at writing actually useful blog posts, I'll publicise a small Python script I wrote to peek inside HDF5 files when <a href="http://www.hdfgroup.org/hdf-java-html/hdfview/">HDFView</a> is overkill. Sometimes you just want to know how many dimensions a stored array has, or its exact path within the HDF hierarchy. 

The “codebase” is currently tiny enough that it all fits below:

```python
#!/usr/bin/env python
 
import os, sys, argparse
import h5py
from numpy import array
 
arguments = argparse.ArgumentParser(add_help=False)
arggroup = arguments.add_argument_group('HDF5 cat options')
arggroup.add_argument('-g', '--group', metavar='GROUP',
    help='Preview only path given by GROUP')
arggroup.add_argument('-v', '--verbose', action='store_true', default=False,
    help='Include array printout.')
 
if __name__ == '__main__':
    parser = argparse.ArgumentParser(
        description='Preview the contents of an HDF5 file',
        parents=[arguments]
    )
    parser.add_argument('fin', nargs='+', help='The input HDF5 files.')
 
    args = parser.parse_args()
    for fin in args.fin:
        print '>>>', fin
        f = h5py.File(fin, 'r')
        if args.group is not None:
            groups = [args.group]
        else:
            groups = []
            f.visit(groups.append)
        for g in groups:
            print '\n   ', g
            if type(f[g]) == h5py.highlevel.Dataset:
                a = f[g]
                print '      shape: ', a.shape, '\n      type: ', a.dtype
                if args.verbose:
                    a = array(f[g])
                    print a
```

h5cat is [available](https://github.com/jni/h5cat) on GitHub under an MIT license. Here's an example use case:

```
$ h5cat -v -g vi single-channel-tr3-0-0.00.lzf.h5
>>> single-channel-tr3-0-0.00.lzf.h5

    vi
      shape:  (3, 1) 
      type:  float64
[[ 0.        ]
 [ 0.06224902]
 [ 2.23062383]]
```
</p></body></html>
