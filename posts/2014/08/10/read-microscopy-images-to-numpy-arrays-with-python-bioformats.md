<!--
.. title: Read microscopy images to numpy arrays with python-bioformats
.. slug: read-microscopy-images-to-numpy-arrays-with-python-bioformats
.. date: 2014-08-10 00:43:36
.. tags: Planet SciPy,Python,Python-Bioformats,programming
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>The python-bioformats library lets you seamlessly read microscopy images into numpy arrays from pretty much any file format.

I <a href="http://ilovesymposia.com/2014/01/15/fiji-jython">recently explained</a> how to use Fiji's Jython interpreter to open BioFormats images, do some processing, and save the result to a standard format such as TIFF. Since then, the
<a href="http://cellprofiler.org">CellProfiler team</a> has released an incredible tool, the <a href="https://github.com/cellprofiler/python-bioformats">python-bioformats</a> library. It uses the Java Native Interface (JNI) to access the Java BioFormats library and give you back a numpy array within Python. In other words, it's magic.

Some of the stuff I was doing in the Jython interpreter was not going to fly for a 400GB image file produced by Leica (namely, setting the flag <code>setOpenAllSeries(True)</code>). This file contained 3D images of multiple zebrafish embryos, obtained every 30 minutes for three days. I needed to process each image sequentially.

The first problem was that even reading the metadata from the file <a href="https://github.com/CellProfiler/python-bioformats/issues/8">resulted in a Java out-of-memory error</a>! With the help of Lee Kamentsky, one of the creators of python-bioformats, I figured out that Java allocates a maximum memory footprint of just 256MB. With the raw metadata string occupying 27MB, this was not enough to contain the full structure of the parsed metadata tree. The solution was simply to set a much larger maximum memory allocation to the JVM:

```https://github.com/jni/lesion/blob/c3223687d35a7f81da7305e1e041f9c5a53104b1/lesion/lifio.py#L79
, cmap=cm.gray)
plt.show()

```

<img src="http://ilovesymposia.files.wordpress.com/2014/08/embryo-image.png" alt="2D slice from 5D volume">

Boom. Using Python BioFormats, I've read in a small(ish) part of a quasi-terabyte image file into a numpy array, ready for further processing.

Note: the dimension order here is time, z, y, x, channel, or TZYXC, which I <em>think</em> is the most efficient way to read these files in. My <a href="https://github.com/jni/lesion/blob/c3223687d35a7f81da7305e1e041f9c5a53104b1/lesion/lifio.py#L181">wrapper</a> allows arbitrary dimension order, so it'll be good to use it to figure out the fastest way to iterate through the volume.

In my case, I'm looking to extract statistics using scikit-image's <a href="https://github.com/scikit-image/scikit-image/blob/master/skimage/measure/profile.py"><code>profile_line</code></a> function, and plot their evolution over time. Here's the min/max intensity <a href="https://github.com/jni/lesion/blob/c3223687d35a7f81da7305e1e041f9c5a53104b1/lesion/trace.py">profile</a> along the embryo for a sample stack:

<img src="http://ilovesymposia.files.wordpress.com/2014/08/position02.png" alt="min/max vs time">

I still need to clean up the code, in particular to detect bad images (no prizes for guessing which timepoint was bad here), but my point for now is that, thanks to Python BioFormats, doing your entire bioimage analysis in Python just got a heck of a lot easier.</p></body></html>
