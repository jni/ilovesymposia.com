<!--
.. title: Get the best of both worlds with Fiji's Jython interpreter
.. slug: fiji-jython
.. date: 2014-02-26 01:21:38
.. tags: Planet SciPy,programming,Research Blogging,software
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><a href="http://fiji.sc">Fiji is just ImageJ</a>, with batteries included. It contains plugins to do virtually anything you would want to do to an image. Since my go-to programming language is Python, my favorite feature of Fiji is its language-agnostic API, which supports a plethora of languages, including Java, Javascript, Clojure, and of course Python; 7 languages in all. (Find these under Plugins/Scripting/Script Editor.) Read on to learn more about the ins and outs of using Python to drive Fiji.

Among the plugin smorgasbord of Fiji is the Bio-Formats importer, which can open any proprietary microscopy file under the sun. (And there’s a lot of them!) Below I will use Jython to open some .lifs, do some processing, and output some .pngs that I can process further using Python/NumPy/scikit-image. (A .lif is a Leica Image File, because there were not enough image file formats before Leica came along.)

The first thing to note is that Jython is not Python, and it is certainly not Python 2.7. In fact, the Fiji Jython interpreter implements Python 2.5, which means no <code>argparse</code>. Not to worry though, as <code>argparse</code> is implemented in a <a href="https://code.google.com/p/argparse/source/browse/argparse.py">single, pure Python file</a> distributed under the Python license. So:

<strong>Tip #1: copy argparse.py into your project.</strong>

This way you’ll have access the state of the art in command line argument processing from within the Jython interpreter.

To get Fiji to run your code, you simply feed it your source file on the command line. So, let’s try it out with a simple example, <code>echo.py</code>:
<pre><code class="python">import argparse

if __name__ == '__main__':
    parser = argparse.ArgumentParser(description=
                                     "Parrot back your arguments.")
    parser.add_argument('args', nargs="*", help="The input arguments.")
    args = parser.parse_args()
    for arg in args.args:
        print(arg)

</code></pre>
Now we can just run this:

<pre><code>$ fiji echo.py hello world
hello
world
</code></pre>

But sadly, Fiji captures any -h calls, which defeats the purpose of using argparse in the first place!

<pre><code>$ fiji echo.py -h
Usage: /Applications/Fiji.app/Contents/MacOS/fiji-macosx [&lt;Java options&gt;.. --] [&lt;ImageJ options&gt;..] [&lt;files&gt;..]

Java options are passed to the Java Runtime, ImageJ
options to ImageJ (or Jython, JRuby, ...).

In addition, the following options are supported by ImageJ:
General options:
--help, -h
	show this help
--dry-run
	show the command line, but do not run anything
--debug
	verbose output
</code></pre>

(… and so on, the output is quite huge.)

(Note also that I aliased the Fiji binary, that long path under <code>/Applications</code>, to a simple <code>fiji</code> command; I recommend you do the same.)

However, we can work around this by calling help using <em>Python</em> as the interpreter, and only using Fiji to actually run the file:

<pre><code>$ python echo.py -h
usage: echo.py [-h] [args [args ...]]

Parrot back your arguments.

positional arguments:
  args        The input arguments.

optional arguments:
  -h, --help  show this help message and exit</code></pre>
That’s more like it! Now we can start to build something a bit more interesting, for example, something that converts arbitrary image files to png:
<pre><code class="python">import argparse
from ij import IJ # the IJ class has utility methods for many common tasks.

def convert_file(fn):
    """Convert the input file to png format.

    Parameters
    ----------
    fn : string
        The filename of the image to be converted.
    """
    imp = IJ.openImage(fn)
    # imp is the common name for an ImagePlus object,
    # ImageJ's base image class
    fnout = fn.rsplit('.', 1)[0] + '.png'
    IJ.saveAs(imp, 'png', fnout)

if __name__ == '__main__':
    parser = argparse.ArgumentParser(description="Convert TIFF to PNG.")
    parser.add_argument('images', nargs='+', help="Input images.")

    args = parser.parse_args()
    for fn in args.images:
        convert_file(fn)
</code></pre>

Boom, we’re done. But wait, we actually broke the Python interpreter compatibility, since ij is not a Python library!

<pre><code>$ python convert2png.py -h
Traceback (most recent call last):
  File "convert.py", line 2, in &lt;module&gt;
    from ij import IJ # the IJ class has utility methods for many common tasks.
ImportError: No module named ij
</code></pre>

Which brings us to:

<strong>Tip #2: only import Java API functions within the functions that use them.</strong>

By moving the <code>from ij import IJ</code> statement into the <code>convert</code> function, we maintain compatibility with Python, and can continue to use <code>argparse</code>’s helpful documentation strings.

Next, we want to use the Bio-Formats importer, which is class <code>BF</code> in <code>loci.plugins</code>. Figuring out the class hierarchy for arbitrary plugins is tricky, but you can find it <a href="http://rsbweb.nih.gov/ij/developer/api/index.html">here</a> for core ImageJ (using lovely 1990s-style frames) and <a href="http://ci.openmicroscopy.org/job/BIOFORMATS-5.0-latest/javadoc/index.html">here</a> for Bio-Formats, and Curtis Rueden has made <a href="http://javadoc.imagej.net/">this list</a> for other common plugins.

When you try to open a file with Bio-Formats importer using the Fiji GUI, you get the following dialog:

<figure><img alt="BioFormats import window" src="http://ilovesymposia.files.wordpress.com/2014/02/bioformats-window.png"> <figcaption>BioFormats import window</figcaption></figure>

That’s a lot of options, and we actually want to set some of them. If you look at the <a href="http://ci.openmicroscopy.org/job/BIOFORMATS-5.0-latest/javadoc/loci/plugins/BF.html#openImagePlus(loci.plugins.in.ImporterOptions)"><code>BF.openImagePlus</code></a> documentation, you can see that this is done through an <code>ImporterOptions</code> class located in <code>loci.plugins.in</code>. You’ll notice that “in” is a reserved word in Python, so <code>from loci.plugins.in import ImporterOptions</code> is not a valid Python statement. Yay! My workaround:

<strong>Tip #3: move your Fiji imports to an external file.</strong>

So I have a <code>jython_imports.py</code> file with just:

<pre><code class="python">from ij import IJ
from loci.plugins import BF
from loci.plugins.in import ImporterOptions
</code></pre>

Then, inside the <code>convert_files()</code> function, we just do:

<pre><code class="python">
from jython_imports import IJ, BF, ImporterOptions
</code></pre>

This way, the main file remains Python-compatible until the convert() function is actually called, regardless of whatever funky and unpythonic stuff is happening in <code>jython_imports.py</code>.

Onto the options. If you untick “Open files individually”, it will open up all matching files in a directory, regardless of your input filename! Not good. So now we play a pattern-matching game in which we match the option description in the above dialog with the <a href="http://ci.openmicroscopy.org/job/BIOFORMATS-5.0-latest/javadoc/loci/plugins/in/ImporterOptions.html">ImporterOptions API</a> calls. In this case, we <code>setUngroupFiles(True)</code>. To specify a filename, we <code>setId(filename)</code>. Additionally, because we want all of the images in the .lif file, we <code>setOpenAllSeries(True)</code>.

Next, each image in the series is 3D and has three channels, but we are only interested in a summed z-projection of the first channel. There’s a set of ImporterOptions methods tantalizingly named <code>setCBegin</code>, <code>setCEnd</code>, and <code>setCStep</code>, but this is where I found the <a href="http://ci.openmicroscopy.org/job/BIOFORMATS-5.0-latest/javadoc/loci/plugins/in/ImporterOptions.html#setCEnd(int,%20int)">documentation</a> sorely lacking. The functions take <code>(int s, int value)</code> as arguments, but what’s <code>s</code>??? Are the limits closed or open? Code review is a wonderful thing, and this would not have passed it. To figure things out:

<strong>Tip #4: use Fiji’s interactive Jython interpreter to figure things out quickly.</strong>

You can find the Jython interpreter under Plugins/Scripting/Jython Interpreter. It’s no IPython, but it is extremely helpful to answer the questions I had above. My hypothesis was that <code>s</code> was the series, and that the intervals would be closed. So:

```python
>>> from loci.plugins import BF
>>> from loci.plugins.in import ImporterOptions
>>> opts = ImporterOptions()
>>> opts.setId("myFile.lif")
>>> opts.setOpenAllSeries(True)
>>> opts.setUngroupFiles(True)
>>> imps = BF.openImagePlus(opts)
```

Now we can play around, with one slight annoyance: the interpreter won’t print the output of your last statement, so you have to specify it:

```python
>>> len(imps)
>>> print(len(imps))
18
```

Which is what I expected, as there are 18 series in my .lif file. The image shape is given by the <code>getDimensions()</code> method of the ImagePlus class:

```python
>>> print(imps[0].getDimensions())
array('i', [1024, 1024, 3, 31, 1])

>>> print(imps[1].getDimensions())
array('i', [1024, 1024, 3, 34, 1])</code></pre>
That’s (x, y, channels, z, time).
```

Now, let’s try the same thing with <code>setCEnd</code>, assuming closed interval:

```python
>>> opts.setCEnd(0, 0) ## only read channels up to 0 for series 0?
>>> opts.setCEnd(2, 0) ## only read channels up to 0 for series 2?
>>> imps = BF.openImagePlus(opts)
>>> print(imps[0].getDimensions())
array('i', [1024, 1024, 1, 31, 1])

>>> print(imps[1].getDimensions())
array('i', [1024, 1024, 3, 34, 1])

>>> print(imps[2].getDimensions())
array('i', [1024, 1024, 1, 30, 1])
```

Nothing there to disprove my hypothesis! So we move on to the final step, which is to z-project the stack by summing the intensity over all z values. This is normally accessed via Image/Stacks/Z Project in the Fiji GUI, and I found the corresponding <code>ij.plugin.ZProjector</code> class by searching for <a href="http://rsbweb.nih.gov/ij/developer/api/ij/plugin/ZProjector.html">“proj” in the ImageJ documentation</a>. A <code>ZProjector</code> object has a <code>setMethod</code> method that usefully takes an int as an argument, with no explanation in its docstring as to which int translates to which method (sum, average, max, etc.). A little more digging in the <a href="http://rsb.info.nih.gov/ij/developer/source/ij/plugin/ZProjector.java.html">source code</a> reveals some class static variables, <code>AVG_METHOD</code>, <code>MAX_METHOD</code>, and so on.

<strong>Tip #5: don’t be afraid to look at the source code. It’s one of the main advantages of working in open-source.</strong>

So:

```python
>>> from ij.plugin import ZProjector
>>> proj = ZProjector()
>>> proj.setMethod(ZProjector.SUM_METHOD)
>>> proj.setImage(imps[0])
>>> proj.doProjection()
>>> impout = proj.getProjection()
>>> print(impout.getDimensions())
array('i', [1024, 1024, 1, 1, 1])
```

The output is actually a float-typed image, which will get rescaled to [0, 255] uint8 on save if we don’t fix it. So, to wrap up, we convert the image to 16 bits (making sure to <a href="https://groups.google.com/d/msg/fiji-users/HfuHj0QBo40/CR9s3MQ5vUsJ">turn off scaling</a>), use the series title to generate a unique filename, and save as a PNG:
<pre><code class="python">

```python
>>> from ij.process import ImageConverter
>>> ImageConverter.setDoScaling(False)
>>> conv = ImageConverter(impout)
>>> conv.convertToGray16()
>>> title = imps[0].getTitle().rsplit(" ", 1)[-1]
>>> IJ.saveAs(impout, 'png', "myFile-" + title + ".png")
```

You can see the final result of my sleuthing in <a href="https://github.com/jni/lesion/blob/6f77cccd1e0f3ddf92ce35a7040ada5328fd90ff/lesion/lif2png.py">lif2png.py</a> and <a href="https://github.com/jni/lesion/blob/6f77cccd1e0f3ddf92ce35a7040ada5328fd90ff/lesion/jython_imports.py">jython_imports.py</a>. If you would do something differently, pull requests are always welcome.

Before I sign off, let me recap my tips:

1. copy argparse.py into your project;

2. only import Java API functions within the functions that use them;

3. move your Fiji imports to an external file;

4. use Fiji’s interactive Jython interpreter to figure things out quickly; and

5. don’t be afraid to look at the source code.

And let me add a few final comments: once I started digging into all of Fiji’s plugins, I found documentation of very variable quality, and worse, virtually zero consistency between the interfaces to each plugin. Some work on “the currently active image”, some take an <code>ImagePlus</code> instance as input, and others still a filename or a directory name. Outputs are equally variable. This has been a huge pain when trying to work with these plugins.

But, on the flipside, this is the most complete collection of image processing functions anywhere. Along with the seamless access to all those functions from Jython and other languages, that makes Fiji very worthy of your attention.
<h3 id="acknowledgements">Acknowledgements</h3>
This post was possible thanks to the help of <a href="http://albert.rierol.net/">Albert Cardona</a>, <a href="http://loci.wisc.edu/people/johannes-schindelin">Johannes Schindelin</a>, <a href="http://loci.wisc.edu/people/wayne-rasband">Wayne Rasband</a>, and <a href="http://lammertlab.org/Jan_Eglinger">Jan Eglinger</a>, who restlessly respond to (it seems) every query on the <a href="http://imagej.1557.x6.nabble.com/">ImageJ mailing list</a>. Thanks!
<h3 id="reference">References</h3>

<span class="Z3988" title="ctx_ver=Z39.88-2004&amp;rft_val_fmt=info%3Aofi%2Ffmt%3Akev%3Amtx%3Ajournal&amp;rft.jtitle=Nature+methods&amp;rft_id=info%3Apmid%2F22743772&amp;rfr_id=info%3Asid%2Fresearchblogging.org&amp;rft.atitle=Fiji%3A+an+open-source+platform+for+biological-image+analysis.&amp;rft.issn=1548-7091&amp;rft.date=2012&amp;rft.volume=9&amp;rft.issue=7&amp;rft.spage=676&amp;rft.epage=82&amp;rft.artnum=&amp;rft.au=Schindelin+J&amp;rft.au=Arganda-Carreras+I&amp;rft.au=Frise+E&amp;rft.au=Kaynig+V&amp;rft.au=Longair+M&amp;rft.au=Pietzsch+T&amp;rft.au=Preibisch+S&amp;rft.au=Rueden+C&amp;rft.au=Saalfeld+S&amp;rft.au=Schmid+B&amp;rft.au=Tinevez+JY&amp;rft.au=White+DJ&amp;rft.au=Hartenstein+V&amp;rft.au=Eliceiri+K&amp;rft.au=Tomancak+P&amp;rft.au=Cardona+A&amp;rfe_dat=bpr3.included=1;bpr3.tags=Biology%2CComputer+Science+%2F+Engineering%2CComputational+Biology%2C+Bioinformatics">Schindelin J, Arganda-Carreras I, Frise E, Kaynig V, Longair M, Pietzsch T, Preibisch S, Rueden C, Saalfeld S, Schmid B, Tinevez JY, White DJ, Hartenstein V, Eliceiri K, Tomancak P, &amp; Cardona A (2012). Fiji: an open-source platform for biological-image analysis. <span style="font-style:italic;">Nature methods, 9</span> (7), 676-82 PMID: <a rev="review" href="http://www.ncbi.nlm.nih.gov/pubmed/22743772">22743772</a></span>

<span class="Z3988" title="ctx_ver=Z39.88-2004&amp;rft_val_fmt=info%3Aofi%2Ffmt%3Akev%3Amtx%3Ajournal&amp;rft.jtitle=The+Journal+of+cell+biology&amp;rft_id=info%3Apmid%2F20513764&amp;rfr_id=info%3Asid%2Fresearchblogging.org&amp;rft.atitle=Metadata+matters%3A+access+to+image+data+in+the+real+world.&amp;rft.issn=0021-9525&amp;rft.date=2010&amp;rft.volume=189&amp;rft.issue=5&amp;rft.spage=777&amp;rft.epage=82&amp;rft.artnum=&amp;rft.au=Linkert+M&amp;rft.au=Rueden+CT&amp;rft.au=Allan+C&amp;rft.au=Burel+JM&amp;rft.au=Moore+W&amp;rft.au=Patterson+A&amp;rft.au=Loranger+B&amp;rft.au=Moore+J&amp;rft.au=Neves+C&amp;rft.au=Macdonald+D&amp;rft.au=Tarkowska+A&amp;rft.au=Sticco+C&amp;rft.au=Hill+E&amp;rft.au=Rossner+M&amp;rft.au=Eliceiri+KW&amp;rft.au=Swedlow+JR&amp;rfe_dat=bpr3.included=1;bpr3.tags=Biology%2CComputer+Science+%2F+Engineering%2CComputational+Biology%2C+Bioinformatics">Linkert M, Rueden CT, Allan C, Burel JM, Moore W, Patterson A, Loranger B, Moore J, Neves C, Macdonald D, Tarkowska A, Sticco C, Hill E, Rossner M, Eliceiri KW, &amp; Swedlow JR (2010). Metadata matters: access to image data in the real world. <span style="font-style:italic;">The Journal of cell biology, 189</span> (5), 777-82 PMID: <a rev="review" href="http://www.ncbi.nlm.nih.gov/pubmed/20513764">20513764</a></span></body></html>
