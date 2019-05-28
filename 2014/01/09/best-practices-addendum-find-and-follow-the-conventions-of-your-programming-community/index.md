<!--
.. title: Best practices addendum: find and follow the conventions of your programming community
.. slug: best-practices-addendum-find-and-follow-the-conventions-of-your-programming-community
.. date: 2014-01-09 21:11:42
.. tags: Planet SciPy,Scientific Computing,programming
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>
The bioinformatics community is all atwitter about the recent PLOS Biology article, <a href="http://www.plosbiology.org/article/info:doi/10.1371/journal.pbio.1001745">Best Practices for Scientific Computing</a>. Its main points should be obvious to most quasi-experienced programmers, but I can certainly remember a time when they did not seem so obvious to me (last week I think). As such, it's a valuable addition to the written record on scientific computing. One of their code snippets, however, is pretty annoying:
</p>

<pre>
def scan(op, values, seed=None):
# Apply a binary operator cumulatively to the values given
# from lowest to highest, returning a list of results.
# For example, if "op" is "add" and "values" is "[1,3,5]",
# the result is "[1, 4, 9]" (i.e., the running total of the
# given values). The result always has the same length as
# the input.
# If "seed" is given, the result is initialized with that
# value instead of with the first item in "values", and
# the final item is omitted from the result.
# Ex : scan(add, [1, 3, 5] , seed=10)
# produces [10, 11, 14]
...implementation...
</pre>

<p>
First, this code ignores the article's own advice, <em>(1b) make names consistent, distinctive, and meaningful. </em> I would argue that "scan" here is neither distinctive (many other operations could be called "scan") nor meaningful (the function purpose is not at all clear from the name). My suggestion would be "cumulative_reduce".
</p>

<!-- TEASER_END -->

<p>
It also does not address another important piece of advice that I would add to their list, maybe as (1d): <strong>Find out, and follow, the conventions of the programming community you're joining.</strong> This will allow others to use and assess your code more readily, and you to contribute to other code libraries more easily. Here, although they have made efforts to make their advice language-agnostic, the authors have chosen Python to illustrate their point. Python happens to have strong style and documentation prescriptions in the form of Python Enhancement Proposals <a href="http://www.python.org/dev/peps/pep-0008/">PEP-8: Style Guide for Python Code</a> and <a href="http://www.python.org/dev/peps/pep-0257/">PEP-257: Docstring conventions</a>. Following PEP-8 and PEP-257, the above comments become an actual docstring (which is attached to the function automatically by documentation-generating tools):
</p>

<pre>
def cumulative_reduce(op, values, seed=None):
    """Apply a binary operator cumulatively to the values given.

    The operator is applied from left to right.

    For example, if "op" is "add" and "values" is "[1,3,5]",
    the result is "[1, 4, 9]" (i.e., the running total of the
    given values). The result always has the same length as
    the input.

    If "seed" is given, the result is initialized with that
    value instead of with the first item in "values", and
    the final item is omitted from the result.
    Ex : scan(add, [1, 3, 5] , seed=10)
    produces [10, 11, 14]
    """
    ...implementation...
</pre>

<p>
In addition, the Scientific Python community in particular has adopted a few docstring conventions of their own, including the <a href="https://github.com/numpy/numpy/blob/master/doc/HOWTO_DOCUMENT.rst.txt">NumPy docstring conventions</a>, which divide the docstring into meaningful sections using ReStructured Text, and the <a href="http://docs.python.org/2/library/doctest.html">doctest</a> convention to format examples, so the documentation acts as unit tests. So, to further refine their example code:
</p>

<pre>
def cumulative_reduce(op, values, seed=None):
    """Apply a binary operator cumulatively to the values given.

    The operator is applied from left to right.

    Parameters
    ----------
    op : binary function
        An operator taking as input to values of the type contained in
        `values` and returning a value of the same type.
    values : list
        The list of input values.
    seed : type contained in `values`, optional
        A seed to start the reduce operation.

    Returns
    -------
    reduced : list, same type as `values`
        The accumulated list.

    Examples
    --------
    &gt;&gt;&gt; add = lambda x, y: x + y
    &gt;&gt;&gt; cumulative_reduce(add, [1, 3, 5])
    [1, 4, 9]

    If "seed" is given, the result is initialized with that
    value instead of with the first item in "values", and
    the final item is omitted from the result.

    &gt;&gt;&gt; cumulative_reduce(add, [1, 3, 5], seed=10)
    [10, 11, 14]
    """
    ...implementation...
</pre>

<p>
Obviously, these conventions are specific to scientific Python. But the key is that other communities will have their own, and you should find out what those conventions are and adopt them. When in Rome, do as the Romans do. It's actually taken me quite a few years of scientific programming to realise this (and internalise it). I hope this post will help someone get up to speed more quickly than I have.
</p>

<p>
(Incidentally, the Wordpress/Chrome/OSX spell checker doesn't bat an eye at "atwitter". That's awesome.)
</p>

<p><strong>Reference</strong></p>

<span class="Z3988" title="ctx_ver=Z39.88-2004&amp;rft_val_fmt=info%3Aofi%2Ffmt%3Akev%3Amtx%3Ajournal&amp;rft.jtitle=PLoS+Biol&amp;rft_id=info%3Adoi%2F10.1371%2Fjournal.pbio.1001745&amp;rfr_id=info%3Asid%2Fresearchblogging.org&amp;rft.atitle=Best+Practices+for+Scientific+Computing&amp;rft.issn=&amp;rft.date=2014&amp;rft.volume=12&amp;rft.issue=1&amp;rft.spage=0&amp;rft.epage=&amp;rft.artnum=http%3A%2F%2Fwww.plosbiology.org%2Farticle%2Finfo%3Adoi%2F10.1371%2Fjournal.pbio.1001745&amp;rft.au=Greg+Wilson&amp;rft.au=DA+Aruliah&amp;rft.au=C+Titus+Brown&amp;rft.au=Neil+P+Chue+Hong&amp;rft.au=Matt+Davis&amp;rft.au=Richard+T+Guy&amp;rft.au=Steven+HD+Haddock&amp;rft.au=Kathryn+D+Huff&amp;rft.au=Ian+M+Mitchell&amp;rft.au=Mark+D+Plumbley&amp;rft.au=Ben+Waugh&amp;rft.au=Ethan+P+White&amp;rft.au=Paul+Wilson&amp;rfe_dat=bpr3.included=1;bpr3.tags=Biology%2CComputer+Science+%2F+Engineering%2CComputational+Biology%2C+Bioinformatics%2C+Software+Engineering">Greg Wilson, DA Aruliah, C Titus Brown, Neil P Chue Hong, Matt Davis, Richard T Guy, Steven HD Haddock, Kathryn D Huff, Ian M Mitchell, Mark D Plumbley, Ben Waugh, Ethan P White, &amp; Paul Wilson (2014). Best Practices for Scientific Computing <span style="font-style:italic;">PLoS Biol, 12</span> (1) DOI: <a rev="review" href="http://dx.doi.org/10.1371/journal.pbio.1001745">10.1371/journal.pbio.1001745</a></span></body></html>
