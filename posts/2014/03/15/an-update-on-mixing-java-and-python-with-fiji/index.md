<!--
.. title: An update on mixing Java and Python with Fiji
.. slug: an-update-on-mixing-java-and-python-with-fiji
.. date: 2014-03-15 02:21:06
.. tags: Fiji,Fiji Jython,Jython,Planet SciPy,Python,Python-Bioformats,Python-Javabridge,programming,software
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>Two weeks ago I <a href="http://ilovesymposia.com/2014/02/26/fiji-jython/">posted</a> about invoking ImageJ functions from Python using Fiji’s Jython interpreter. A couple of updates on the topic: </p>

<p>First, I’ve made a <a href="https://github.com/jni/fiji-python">repository</a> with a template project encapsulating my tips from that post. It’s very simple to get a Fiji Jython script working from that template. As an example, <a href="https://github.com/jni/snemi-eval">here’s</a> a script to evaluate segmentations using the metric used by the <a href="brainiac2.mit.edu/SNEMI3D/">SNEMI3D segmentation challenge</a> (a slightly modified version of the adapted Rand error). </p>

<p>Second, this entire discussion might be rendered obsolete by two incredible projects from the <a href="http://www.cellprofiler.org/">CellProfiler</a> team: <a href="https://github.com/CellProfiler/python-javabridge">Python-Javabridge</a>, which allows Python to interact seamlessly with Java code, and <a href="https://github.com/CellProfiler/python-bioformats">Python-Bioformats</a>, which uses Python-Javabridge to read Bioformats images into Python. I have yet to play with them, but both look like cleaner alternatives to interact with ImageJ than my Jython scripting! At some point I’ll write a post exploring these tools, but if you get to it before me, please mention it in the comments!</p></body></html>
