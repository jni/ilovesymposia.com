<!--
.. title: Numba in the real world
.. slug: numba-in-the-real-world
.. date: 2016-12-20 21:03:58
.. tags: Numba,open-source,Planet SciPy,Python,Scientific Computing,SciPy,programming
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><div class="cell border-box-sizing text_cell rendered">
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
Numba is a just-in-time compiler (JIT) for Python code focused on NumPy arrays and scientific Python. I've seen various tutorials around the web and in conferences, but I have yet to see someone use Numba "in the wild". In the past few months, I've been using Numba in my own code, and I recently released my first real package using Numba, <a href="https://pypi.python.org/pypi/skan">skan</a>. The short version is that Numba is <em>amazing</em> and you should strongly consider it to speed up your scientific Python bottlenecks. Read on for the longer version.

</div>
</div>
</div>

<!-- TEASER_END -->

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Part-1:-some-toy-examples">Part 1: some toy examples<a class="anchor-link" href="-some-toy-examples">¶</a></h2>
</div>
</div>
</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
Let me illustrate what Numba is good for with the most basic example: adding two arrays together. You've probably seen similar examples around the web.

We start by defining a pure Python function for iterating over a pair of arrays and adding them:

</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [1]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">numpy</span> <span class="k">as</span> <span class="nn">np</span>

<span class="k">def</span> <span class="nf">addarr</span><span class="p">(</span><span class="n">x</span><span class="p">,</span> <span class="n">y</span><span class="p">):</span>
    <span class="n">result</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">zeros_like</span><span class="p">(</span><span class="n">x</span><span class="p">)</span>
    <span class="k">for</span> <span class="n">i</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="n">x</span><span class="o">.</span><span class="n">size</span><span class="p">):</span>
        <span class="n">result</span><span class="p">[</span><span class="n">i</span><span class="p">]</span> <span class="o">=</span> <span class="n">x</span><span class="p">[</span><span class="n">i</span><span class="p">]</span> <span class="o">+</span> <span class="n">y</span><span class="p">[</span><span class="n">i</span><span class="p">]</span>
    <span class="k">return</span> <span class="n">result</span>
</pre></div>

</div>
</div>
</div>

</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
How long does this take in pure Python?

</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [2]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">n</span> <span class="o">=</span> <span class="nb">int</span><span class="p">(</span><span class="mi">1</span><span class="n">e6</span><span class="p">)</span>
<span class="n">a</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">rand</span><span class="p">(</span><span class="n">n</span><span class="p">)</span>
<span class="n">b</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">rand</span><span class="p">(</span><span class="n">n</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [3]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">%</span><span class="k">timeit</span> -r 1 -n 1 addarr(a, b)
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>1 loop, best of 1: 721 ms per loop
</pre>
</div>
</div>

</div>
</div>

</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
About half a second on my machine. Let's try with Numba using its JIT decorator:

</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [4]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">numba</span>

<span class="n">addarr_nb</span> <span class="o">=</span> <span class="n">numba</span><span class="o">.</span><span class="n">jit</span><span class="p">(</span><span class="n">addarr</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [5]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">%</span><span class="k">timeit</span> -r 1 -n 1 addarr_nb(a, b)
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>1 loop, best of 1: 283 ms per loop
</pre>
</div>
</div>

</div>
</div>

</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
The first time it runs, it's only a tiny bit faster. That's because of the nature of JITs: they only compile code <em>as it is being run</em>, in order to use object type information of the objects passed into the function. (Note that, in Python, the arguments <code>a</code> and <code>b</code> to <code>addarr</code> could be anything: an array, as expected, but also a list, a tuple, even a <code>Banana</code>, if you've defined such a class, and the meaning of the function body is different for each of those types.)

Let's see what happens the next time we run it:

</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [6]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">%</span><span class="k">timeit</span> -r 1 -n 1 addarr_nb(a, b)
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>1 loop, best of 1: 6.36 ms per loop
</pre>
</div>
</div>

</div>
</div>

</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
Whoa! Now the code takes 5ms, about 100 times faster than the pure Python version. And the NumPy equivalent?

</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [7]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">%</span><span class="k">timeit</span> -r 1 -n 1 a + b
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>1 loop, best of 1: 5.62 ms per loop
</pre>
</div>
</div>

</div>
</div>

</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
Only marginally faster than Numba, even though NumPy addition is implemented in highly optimised C code. And, for some data types, Numba even beats NumPy:

</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [8]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">r</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">randint</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="mi">128</span><span class="p">,</span> <span class="n">size</span><span class="o">=</span><span class="n">n</span><span class="p">)</span><span class="o">.</span><span class="n">astype</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">uint8</span><span class="p">)</span>
<span class="n">s</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">random</span><span class="o">.</span><span class="n">randint</span><span class="p">(</span><span class="mi">0</span><span class="p">,</span> <span class="mi">128</span><span class="p">,</span> <span class="n">size</span><span class="o">=</span><span class="n">n</span><span class="p">)</span><span class="o">.</span><span class="n">astype</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">uint8</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [9]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">%</span><span class="k">timeit</span> -r 1 -n 1 r + s
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>1 loop, best of 1: 2.92 ms per loop
</pre>
</div>
</div>

</div>
</div>

</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [10]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">%</span><span class="k">timeit</span> -r 1 -n 1 addarr_nb(r, s)
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>1 loop, best of 1: 238 ms per loop
</pre>
</div>
</div>

</div>
</div>

</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [11]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">%</span><span class="k">timeit</span> -r 1 -n 1 addarr_nb(r, s)
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>1 loop, best of 1: 234 µs per loop
</pre>
</div>
</div>

</div>
</div>

</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
WOW! For smaller data types, Numba beats NumPy by over 10x!

I'm only speculating, but since my clock speed is about 1GHz (I'm writing this on a base Macbook with a 1.1GHz Core-m processor), I suspect that Numba is taking advantage of some <a href="https://en.wikipedia.org/wiki/SIMD">SIMD</a> capabilities of the processor, whereas NumPy is treating each array element as an individual arithmetic operation. (If any Numba or NumPy devs are reading this and have more concrete implementation details that explain this, please share them in the comments!)

</div>
</div>
</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
So hopefully I've got your attention now. For years, NumPy has been the go-to library for performance Python in scientific computing. But, if you wanted to do something a little out of the ordinary, you were stuck. Now, Numba generally matches that for arbitrary code and sometimes beats it handily!

In this context, I decided to use Numba to do something a little less trivial, as part of my research.

</div>
</div>
</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Part-2:-Real-Numba">Part 2: Real Numba<a class="anchor-link" href="-Real-Numba">¶</a></h2>
</div>
</div>
</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
I'll present below a slightly simplified version of the code present in my library, <a href="https://pypi.python.org/pypi/skan">skan</a>, which is currently available on PyPI and conda-forge. The task is to build an graph out of the pixels of a skleton image, like this one:

</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [12]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">%</span><span class="k">matplotlib</span> inline
</pre></div>

</div>
</div>
</div>

</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [13]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">matplotlib.pyplot</span> <span class="k">as</span> <span class="nn">plt</span>
<span class="n">plt</span><span class="o">.</span><span class="n">rcParams</span><span class="p">[</span><span class="s1">'image.cmap'</span><span class="p">]</span> <span class="o">=</span> <span class="s1">'gray'</span>
<span class="n">plt</span><span class="o">.</span><span class="n">rcParams</span><span class="p">[</span><span class="s1">'image.interpolation'</span><span class="p">]</span> <span class="o">=</span> <span class="s1">'nearest'</span>
</pre></div>

</div>
</div>
</div>

</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [14]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">skeleton</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">([[</span><span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">],</span>
                     <span class="p">[</span><span class="mi">0</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">0</span><span class="p">],</span>
                     <span class="p">[</span><span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">0</span><span class="p">],</span>
                     <span class="p">[</span><span class="mi">0</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">0</span><span class="p">],</span>
                     <span class="p">[</span><span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">0</span><span class="p">]],</span> <span class="n">dtype</span><span class="o">=</span><span class="nb">bool</span><span class="p">)</span>
<span class="n">skeleton</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">pad</span><span class="p">(</span><span class="n">skeleton</span><span class="p">,</span> <span class="n">pad_width</span><span class="o">=</span><span class="mi">1</span><span class="p">,</span> <span class="n">mode</span><span class="o">=</span><span class="s1">'constant'</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [15]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">fig</span><span class="p">,</span> <span class="n">ax</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">subplots</span><span class="p">(</span><span class="n">figsize</span><span class="o">=</span><span class="p">(</span><span class="mi">5</span><span class="p">,</span> <span class="mi">5</span><span class="p">))</span>
<span class="n">ax</span><span class="o">.</span><span class="n">imshow</span><span class="p">(</span><span class="n">skeleton</span><span class="p">)</span>
<span class="n">ax</span><span class="o">.</span><span class="n">set_title</span><span class="p">(</span><span class="s1">'Skeleton'</span><span class="p">)</span>
<span class="n">ax</span><span class="o">.</span><span class="n">set_xlabel</span><span class="p">(</span><span class="s1">'col'</span><span class="p">)</span>
<span class="n">ax</span><span class="o">.</span><span class="n">set_ylabel</span><span class="p">(</span><span class="s1">'row'</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area"><div class="prompt output_prompt">Out[15]:</div>

<div class="output_text output_subarea output_execute_result">
<pre></pre>
</div>

</div>

<div class="output_area"><div class="prompt"></div>

<div class="output_png output_subarea ">
<img src="/2016/12/img0.png">
</div>

</div>

</div>
</div>

</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
Every white pixel in the image will be a node in our graph, and we place edges between nodes if the pixels are next to each other (counting diagonals). A natural way to represent a graph in the SciPy world is as a sparse matrix A: we number the nonzero pixels from 1 onwards — these are the rows of the matrix — and then place a 1 at entry A(i, j) when pixel i is adjacent to pixel j. SciPy's <code>sparse.coo_matrix</code> format make it very easy to construct such a matrix: we just need an array with the row coordinates and another with the column coordinates.

Because NumPy arrays are not dynamically resizable like Python lists, it helps to know ahead of time how many edges we are going to need to put in our row and column arrays. Thankfully, a well-known theorem of graph theory states that the number of edges of a graph is half the sum of the degrees. In our case, because we want to add the edges twice (once from i to j and once from j to i, we just need the sum of the degrees exactly. We can find this out with a convolution using <code>scipy.ndimage</code>:

</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [16]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">scipy</span> <span class="k">import</span> <span class="n">ndimage</span> <span class="k">as</span> <span class="n">ndi</span>

<span class="n">neighbors</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">([[</span><span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">],</span>
                      <span class="p">[</span><span class="mi">1</span><span class="p">,</span> <span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">],</span>
                      <span class="p">[</span><span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">1</span><span class="p">]])</span>

<span class="n">degrees</span> <span class="o">=</span> <span class="n">ndi</span><span class="o">.</span><span class="n">convolve</span><span class="p">(</span><span class="n">skeleton</span><span class="o">.</span><span class="n">astype</span><span class="p">(</span><span class="nb">int</span><span class="p">),</span> <span class="n">neighbors</span><span class="p">)</span> <span class="o">*</span> <span class="n">skeleton</span>
</pre></div>

</div>
</div>
</div>

</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [17]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">fig</span><span class="p">,</span> <span class="n">ax</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">subplots</span><span class="p">(</span><span class="n">figsize</span><span class="o">=</span><span class="p">(</span><span class="mi">5</span><span class="p">,</span> <span class="mi">5</span><span class="p">))</span>
<span class="n">result</span> <span class="o">=</span> <span class="n">ax</span><span class="o">.</span><span class="n">imshow</span><span class="p">(</span><span class="n">degrees</span><span class="p">,</span> <span class="n">cmap</span><span class="o">=</span><span class="s1">'magma'</span><span class="p">)</span>
<span class="n">ax</span><span class="o">.</span><span class="n">set_title</span><span class="p">(</span><span class="s1">'Skeleton, colored by node degree'</span><span class="p">)</span>
<span class="n">ax</span><span class="o">.</span><span class="n">set_xlabel</span><span class="p">(</span><span class="s1">'col'</span><span class="p">)</span>
<span class="n">ax</span><span class="o">.</span><span class="n">set_ylabel</span><span class="p">(</span><span class="s1">'row'</span><span class="p">)</span>
<span class="n">cbar</span> <span class="o">=</span> <span class="n">fig</span><span class="o">.</span><span class="n">colorbar</span><span class="p">(</span><span class="n">result</span><span class="p">,</span> <span class="n">ax</span><span class="o">=</span><span class="n">ax</span><span class="p">,</span> <span class="n">shrink</span><span class="o">=</span><span class="mf">0.7</span><span class="p">)</span>
<span class="n">cbar</span><span class="o">.</span><span class="n">set_ticks</span><span class="p">([</span><span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">2</span><span class="p">,</span> <span class="mi">3</span><span class="p">])</span>
<span class="n">cbar</span><span class="o">.</span><span class="n">set_label</span><span class="p">(</span><span class="s1">'degree'</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area"><div class="prompt"></div>

<div class="output_png output_subarea ">
<img src="/2016/12/img1.png">
</div>

</div>

</div>
</div>

</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
There you can see "tips" of the skeleton, with only 1 neighbouring pixel, as purple, "paths", with 2 neighbours, as red, and "junctions", with 3 neighbors, as yellow.

Now, consider the pixel at position (1, 6). It has two neighbours (as indicated by its colour): (2, 5) and (1, 7). If we number the nonzero pixels as 1, 2, ..., n from left to right and top to bottom, then this pixel has label 2, and its neighbours have labels 6 and 3. We therefore need to add edges (2, 3) and (2, 6) to the graph. Similarly, when we consider pixel 6, we will add edges (6, 5), (6, 3), and (6, 8).

</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [18]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">fig</span><span class="p">,</span> <span class="n">ax</span> <span class="o">=</span> <span class="n">plt</span><span class="o">.</span><span class="n">subplots</span><span class="p">(</span><span class="n">figsize</span><span class="o">=</span><span class="p">(</span><span class="mi">5</span><span class="p">,</span> <span class="mi">5</span><span class="p">))</span>
<span class="n">result</span> <span class="o">=</span> <span class="n">ax</span><span class="o">.</span><span class="n">imshow</span><span class="p">(</span><span class="n">degrees</span><span class="p">,</span> <span class="n">cmap</span><span class="o">=</span><span class="s1">'magma'</span><span class="p">)</span>
<span class="n">cbar</span> <span class="o">=</span> <span class="n">fig</span><span class="o">.</span><span class="n">colorbar</span><span class="p">(</span><span class="n">result</span><span class="p">,</span> <span class="n">ax</span><span class="o">=</span><span class="n">ax</span><span class="p">,</span> <span class="n">shrink</span><span class="o">=</span><span class="mf">0.7</span><span class="p">)</span>
<span class="n">cbar</span><span class="o">.</span><span class="n">set_ticks</span><span class="p">([</span><span class="mi">0</span><span class="p">,</span> <span class="mi">1</span><span class="p">,</span> <span class="mi">2</span><span class="p">,</span> <span class="mi">3</span><span class="p">])</span>

<span class="n">nnz</span> <span class="o">=</span> <span class="nb">len</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">flatnonzero</span><span class="p">(</span><span class="n">degrees</span><span class="p">))</span>
<span class="n">pixel_labels</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">arange</span><span class="p">(</span><span class="n">nnz</span><span class="p">)</span> <span class="o">+</span> <span class="mi">1</span>
<span class="k">for</span> <span class="n">lab</span><span class="p">,</span> <span class="n">y</span><span class="p">,</span> <span class="n">x</span> <span class="ow">in</span> <span class="nb">zip</span><span class="p">(</span><span class="n">pixel_labels</span><span class="p">,</span> <span class="o">*</span><span class="n">np</span><span class="o">.</span><span class="n">nonzero</span><span class="p">(</span><span class="n">degrees</span><span class="p">)):</span>
    <span class="n">ax</span><span class="o">.</span><span class="n">text</span><span class="p">(</span><span class="n">x</span><span class="p">,</span> <span class="n">y</span><span class="p">,</span> <span class="n">lab</span><span class="p">,</span> <span class="n">horizontalalignment</span><span class="o">=</span><span class="s1">'center'</span><span class="p">,</span>
            <span class="n">verticalalignment</span><span class="o">=</span><span class="s1">'center'</span><span class="p">)</span>

<span class="n">ax</span><span class="o">.</span><span class="n">set_xlabel</span><span class="p">(</span><span class="s1">'col'</span><span class="p">)</span>
<span class="n">ax</span><span class="o">.</span><span class="n">set_ylabel</span><span class="p">(</span><span class="s1">'row'</span><span class="p">)</span>
<span class="n">ax</span><span class="o">.</span><span class="n">set_title</span><span class="p">(</span><span class="s1">'Skeleton, with pixel IDs'</span><span class="p">)</span>
<span class="n">cbar</span><span class="o">.</span><span class="n">set_label</span><span class="p">(</span><span class="s1">'degree'</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area"><div class="prompt"></div>

<div class="output_png output_subarea ">
<img src="/2016/12/img2.png">
</div>

</div>

</div>
</div>

</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
Scanning over the whole image, we see that we need <code>row</code> and <code>col</code> arrays of length exactly <code>np.sum(degrees)</code>.

</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [19]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">n_edges</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">sum</span><span class="p">(</span><span class="n">degrees</span><span class="p">)</span>
<span class="n">row</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">empty</span><span class="p">(</span><span class="n">n_edges</span><span class="p">,</span> <span class="n">dtype</span><span class="o">=</span><span class="n">np</span><span class="o">.</span><span class="n">int32</span><span class="p">)</span>  <span class="c1"># type expected by scipy.sparse</span>
<span class="n">col</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">empty</span><span class="p">(</span><span class="n">n_edges</span><span class="p">,</span> <span class="n">dtype</span><span class="o">=</span><span class="n">np</span><span class="o">.</span><span class="n">int32</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
The final piece of the puzzle is finding neighbours. For this, we need to know a little about how NumPy stores arrays. Even though our array is 2-dimensional (rows and columns), these are all arrayed in a giant line, each row placed one after the other. (This is called "C-order".) If we index into this linearised array ("raveled", in NumPy's language), we can make sure that our code works for 2D, 3D, and even higher-dimensional images. Using this indexing, neighbouring pixels to the left and right are accessed by subtracting or adding 1 to the current index. Neighbouring pixels above and below are accessed by subtracting or adding the length of a whole row. Finally, diagonal neighbours are found by combining these two. For simplicity, we only show the 2D version below:

</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [20]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">neighbour_steps</span><span class="p">(</span><span class="n">shape</span><span class="p">):</span>
    <span class="n">step_sizes</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">cumprod</span><span class="p">((</span><span class="mi">1</span><span class="p">,)</span> <span class="o">+</span> <span class="n">shape</span><span class="p">[</span><span class="o">-</span><span class="mi">1</span><span class="p">:</span><span class="mi">0</span><span class="p">:</span><span class="o">-</span><span class="mi">1</span><span class="p">])</span>
    <span class="n">axis_steps</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">array</span><span class="p">([[</span><span class="o">-</span><span class="mi">1</span><span class="p">,</span> <span class="o">-</span><span class="mi">1</span><span class="p">],</span>
                           <span class="p">[</span><span class="o">-</span><span class="mi">1</span><span class="p">,</span>  <span class="mi">1</span><span class="p">],</span>
                           <span class="p">[</span> <span class="mi">1</span><span class="p">,</span> <span class="o">-</span><span class="mi">1</span><span class="p">],</span>
                           <span class="p">[</span> <span class="mi">1</span><span class="p">,</span>  <span class="mi">1</span><span class="p">]])</span>
    <span class="n">diag</span> <span class="o">=</span> <span class="n">axis_steps</span> <span class="o">@</span> <span class="n">step_sizes</span>
    <span class="n">steps</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">concatenate</span><span class="p">((</span><span class="n">step_sizes</span><span class="p">,</span> <span class="o">-</span><span class="n">step_sizes</span><span class="p">,</span> <span class="n">diag</span><span class="p">))</span>
    <span class="k">return</span> <span class="n">steps</span>
</pre></div>

</div>
</div>
</div>

</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [21]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">steps</span> <span class="o">=</span> <span class="n">neighbour_steps</span><span class="p">(</span><span class="n">degrees</span><span class="o">.</span><span class="n">shape</span><span class="p">)</span>
<span class="nb">print</span><span class="p">(</span><span class="n">steps</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>[  1   9  -1  -9 -10   8  -8  10]
</pre>
</div>
</div>

</div>
</div>

</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
Of course, if we use these steps near the right edge of the image, we'll wrap around, and mistakenly think that the first element of the next row is a neighbouring pixel! Our solution is to only process nonzero pixels, and make sure that we have a 1-pixel-wide "pad" of zero pixels — which we do, in the image above!

Now, we iterate over image pixels, look at neighbors, and populate the <code>row</code> and <code>column</code> vectors.

</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [22]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="k">def</span> <span class="nf">build_graph</span><span class="p">(</span><span class="n">labeled_pixels</span><span class="p">,</span> <span class="n">steps_to_neighbours</span><span class="p">,</span> <span class="n">row</span><span class="p">,</span> <span class="n">col</span><span class="p">):</span>
    <span class="n">start</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">max</span><span class="p">(</span><span class="n">steps_to_neighbours</span><span class="p">)</span>
    <span class="n">end</span> <span class="o">=</span> <span class="nb">len</span><span class="p">(</span><span class="n">labeled_pixels</span><span class="p">)</span> <span class="o">-</span> <span class="n">start</span>
    <span class="n">elem</span> <span class="o">=</span> <span class="mi">0</span>  <span class="c1"># row/col index</span>
    <span class="k">for</span> <span class="n">k</span> <span class="ow">in</span> <span class="nb">range</span><span class="p">(</span><span class="n">start</span><span class="p">,</span> <span class="n">end</span><span class="p">):</span>
        <span class="n">i</span> <span class="o">=</span> <span class="n">labeled_pixels</span><span class="p">[</span><span class="n">k</span><span class="p">]</span>
        <span class="k">if</span> <span class="n">i</span> <span class="o">!=</span> <span class="mi">0</span><span class="p">:</span>
            <span class="k">for</span> <span class="n">s</span> <span class="ow">in</span> <span class="n">steps</span><span class="p">:</span>
                <span class="n">neighbour</span> <span class="o">=</span> <span class="n">k</span> <span class="o">+</span> <span class="n">s</span>
                <span class="n">j</span> <span class="o">=</span> <span class="n">labeled_pixels</span><span class="p">[</span><span class="n">neighbour</span><span class="p">]</span>
                <span class="k">if</span> <span class="n">j</span> <span class="o">!=</span> <span class="mi">0</span><span class="p">:</span>
                    <span class="n">row</span><span class="p">[</span><span class="n">elem</span><span class="p">]</span> <span class="o">=</span> <span class="n">i</span>
                    <span class="n">col</span><span class="p">[</span><span class="n">elem</span><span class="p">]</span> <span class="o">=</span> <span class="n">j</span>
                    <span class="n">elem</span> <span class="o">+=</span> <span class="mi">1</span>
</pre></div>

</div>
</div>
</div>

</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [23]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">skeleton_int</span> <span class="o">=</span> <span class="n">np</span><span class="o">.</span><span class="n">ravel</span><span class="p">(</span><span class="n">skeleton</span><span class="o">.</span><span class="n">astype</span><span class="p">(</span><span class="n">np</span><span class="o">.</span><span class="n">int32</span><span class="p">))</span>
<span class="n">skeleton_int</span><span class="p">[</span><span class="n">np</span><span class="o">.</span><span class="n">nonzero</span><span class="p">(</span><span class="n">skeleton_int</span><span class="p">)]</span> <span class="o">=</span> <span class="mi">1</span> <span class="o">+</span> <span class="n">np</span><span class="o">.</span><span class="n">arange</span><span class="p">(</span><span class="n">nnz</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [24]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">%</span><span class="k">timeit</span> -r 1 -n 1 build_graph(skeleton_int, steps, row, col)
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>1 loop, best of 1: 917 µs per loop
</pre>
</div>
</div>

</div>
</div>

</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
Now we try the Numba version:

</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [25]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="n">build_graph_nb</span> <span class="o">=</span> <span class="n">numba</span><span class="o">.</span><span class="n">jit</span><span class="p">(</span><span class="n">build_graph</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [26]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">%</span><span class="k">timeit</span> -r 1 -n 1 build_graph_nb(skeleton_int, steps, row, col)
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>1 loop, best of 1: 346 ms per loop
</pre>
</div>
</div>

</div>
</div>

</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [27]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="o">%</span><span class="k">timeit</span> -r 1 -n 1 build_graph_nb(skeleton_int, steps, row, col)
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area"><div class="prompt"></div>
<div class="output_subarea output_stream output_stdout output_text">
<pre>1 loop, best of 1: 14.3 µs per loop
</pre>
</div>
</div>

</div>
</div>

</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
Nice! We get more than a 50-fold speedup using Numba, and this operation would have been difficult if not impossible to convert to a NumPy vectorized operation! We can now build our graph:

</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [28]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">from</span> <span class="nn">scipy</span> <span class="k">import</span> <span class="n">sparse</span>
<span class="n">G</span> <span class="o">=</span> <span class="n">sparse</span><span class="o">.</span><span class="n">coo_matrix</span><span class="p">((</span><span class="n">np</span><span class="o">.</span><span class="n">ones_like</span><span class="p">(</span><span class="n">row</span><span class="p">),</span> <span class="p">(</span><span class="n">row</span><span class="p">,</span> <span class="n">col</span><span class="p">)))</span><span class="o">.</span><span class="n">tocsr</span><span class="p">()</span>
</pre></div>

</div>
</div>
</div>

</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
As to what to <em>do</em> with said graph, I'll leave that for another post. (You can also peruse the <a href="https://github.com/jni/skan">skan source code</a>.) In the meantime, though, you can visualize it with NetworkX:

</div>
</div>
</div>

<div class="cell border-box-sizing code_cell rendered">
<div class="input">
<div class="prompt input_prompt">In [29]:</div>
<div class="inner_cell">
    <div class="input_area">
<div class=" highlight hl-ipython3"><pre><span></span><span class="kn">import</span> <span class="nn">networkx</span> <span class="k">as</span> <span class="nn">nx</span>

<span class="n">Gnx</span> <span class="o">=</span> <span class="n">nx</span><span class="o">.</span><span class="n">from_scipy_sparse_matrix</span><span class="p">(</span><span class="n">G</span><span class="p">)</span>
<span class="n">Gnx</span><span class="o">.</span><span class="n">remove_node</span><span class="p">(</span><span class="mi">0</span><span class="p">)</span>

<span class="n">nx</span><span class="o">.</span><span class="n">draw_spectral</span><span class="p">(</span><span class="n">Gnx</span><span class="p">,</span> <span class="n">with_labels</span><span class="o">=</span><span class="kc">True</span><span class="p">)</span>
</pre></div>

</div>
</div>
</div>

<div class="output_wrapper">
<div class="output">

<div class="output_area"><div class="prompt"></div>

<div class="output_png output_subarea ">
<img src="/2016/12/img3.png">
</div>

</div>

</div>
</div>

</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
There's our pixel graph! Obviously, the speedup and n-d support are important for bigger, 3D volumes, not for this tiny graph. But they <em>are</em> important, and, thanks to Numba, easy to obtain.

</div>
</div>
</div>

<div class="cell border-box-sizing text_cell rendered">
<div class="prompt input_prompt">
</div>
<div class="inner_cell">
<div class="text_cell_render border-box-sizing rendered_html">
<h2 id="Conclusion">Conclusion<a class="anchor-link" href="#Conclusion">¶</a></h2>I hope I've piqued your interest in Numba and encouraged you to use it in your own projects. I think the future of success of Python in science heavily depends on JITs, and Numba is a strong contender to be the default JIT in this field.

<b><i>Note:</i></b>This post was written using Jupyter Notebook. You can find the source notebook <a href="https://github.com/jni/blog-notebooks/blob/master/complete/Numba%20in%20the%20real%20world.ipynb">here</a>.
</div>
</div>
</div></body></html>
