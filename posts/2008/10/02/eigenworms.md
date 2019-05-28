<!--
.. title: Eigenworms
.. slug: eigenworms
.. date: 2008-10-02 14:12:00
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><span style="float:left;padding:5px;"><a href="http://www.researchblogging.org"><img src="http://www.researchblogging.org/public/citation_icons/rb2_large_gray.png" alt="ResearchBlogging.org"></a></span>

In one of the best papers in computational biology that I have <em>ever</em> seen, <a href="http://www.ploscompbiol.org/article/info:doi/10.1371/journal.pcbi.1000028">Greg Stephens and colleagues have analysed the movements of nematode worms</a>, and found that they can be decomposed into just four fundamental shapes: virtually any shape that the worm can take is a combination of these four shapes.

.. TEASER_END

I'm surprised that no one at <a href="http://www.researchblogging.org">ResearchBlogging.org</a> picked up on it. I suppose the title seems innocuous enough: "Dimensionality and dynamics in the behavior of C. elegans." I suppose that, had I read just that title, I too would have overlooked it.

Thankfully I subscribe to <a href="http://www.f1000biology.com/browse/">Faculty of 1000</a>, the world's largest journal club. This paper was marked "Exceptional," the highest rating available, by Leonard Maler, of the University of Ottawa. I'll reprint the first sentence of his review because it sums up the paper (and what makes it so brilliant) so nicely:
<blockquote>In this intriguing paper, the apparent random wiggling of a worm (C. elegans) was decomposed into a small set of invariant "wiggles", whimsically termed "eigenworms" by the authors.</blockquote>
<!--more-->

For those of you that haven't been keeping up with their linear algebra, "eigen" is German and roughly translates to "characteristic." The prefix is most commonly used when referring to the eigenvectors and eigenvalues of a matrix.

Let me break that down a bit further: a vector is a column of numbers, and how many of them there are is called the <em>dimension</em> of the vector; a matrix is a set of numbers arranged in rows and columns.

The authors of the paper began by describing a worm's shape as a vector of 100 angles: each worm is artificially divided into 101 segments and then its shape is automatically described by the 100 angles between adjacent segments. They gathered data on thousands of worms, producing thousands of 100-dimensional vectors that define the recorded shape of a particular worm. With all these vectors, one can ask: are any two angles correlated? For example, when angle #5 is 3 degrees to the left, does that tell us anything about angle #6? Of course it does, but we can make it mathematically precise: we can define a <em>correlation matrix</em> of 100 rows by 100 columns, in which an entry on the diagonal is always 1 (because an angle can always predict its own value with 100% accuracy), and the entry on row i and column j defines the correlation between angles i and j.

Now, every square matrix (same number of rows and columns) has at least one <em>eigenvector</em> and a corresponding <em>eigenvalue</em>. This is a vector which, when multiplied by the matrix (we won't get into <a href="http://en.wikipedia.org/wiki/Matrix_multiplication">matrix multiplication</a> here) gives itself, multiplied by a simple number, called its <em>eigenvalue</em>.

Now, n-dimensional symmetric matrices, such as our correlation matrix mentioned above (let's call it C), can actually be decomposed into a product of three matrices: C = QAQ', where Q is a matrix in which each column is one of the n eigenvectors, A is a matrix of all 0s, except the n entries on the diagonal, which are the eigenvalues (in the same order as the eigenvectors are in Q—usually sorted from largest to smallest eigenvalue), and Q' is the same as Q except the rows and columns are interchanged.

Now, the magic happens: if some of the eigenvalues are very large compared to the rest, then we can set the small eigenvalues (equivalently, their corresponding eigenvectors) to 0, and you will still get something close to the original C! That is, C ~= QBQ', where B is the same as A with the small eigenvalues set to 0.

In the case of the Eigenworm paper, the authors found that just four of the 100 eigenvalues accounted for more than 90% of the variability of worm shapes! By extension, it's just four eigenvectors of C that account for this variability: these vectors define four worm shapes: eigenworms! Again: 90% of any worm's shape is defined by a combination of just <em>four</em> shapes. (Imagine the same thing for humans!)

The authors then exploit this property to predict worms' dynamics and movement based on the possible combinations of these shapes. They predict the <em>role</em> of each of the eigenworms in worm navigation: the first two correspond to a wave propagating along a worm's body, and thus contribute to forward motion. The third one corresponds to curvature, and therefore variations in the contribution of this eigenworm are responsible for the worm turning in its trajectory. Finally, the fourth eigenworm defines movement of the worm's head as it forages and navigates.

In a final coup-de-grace in an already stellar paper, Stephens <em>et al.</em> test their predictions by defining how movement in the space defined by the eigenworms translates to worms' movement. This test they pass with flying colours, with the worms turning precisely when the third eigenworm would predict, and moving at the speed that the first and second eigenworms would.

I suppose one might think, big deal, so they can tell how a worm moves. But this is mathematics describing almost everything about a biological phenomenon, and all in the swift swoop of a single paper. It doesn't get any better than this.

<span class="Z3988" title="ctx_ver=Z39.88-2004&amp;rft_val_fmt=info%3Aofi%2Ffmt%3Akev%3Amtx%3Ajournal&amp;rft.jtitle=PLoS+Computational+Biology&amp;rft.id=info%3ADOI%2F10.1371%2Fjournal.pcbi.1000028&amp;rft.atitle=Dimensionality+and+Dynamics+in+the+Behavior+of+C.+elegans&amp;rft.date=2008&amp;rft.volume=4&amp;rft.issue=4&amp;rft.spage=0&amp;rft.epage=0&amp;rft.artnum=http%3A%2F%2Fdx.plos.org%2F10.1371%2Fjournal.pcbi.1000028&amp;rft.au=Greg+J.+Stephens&amp;rft.au=Bethany+Johnson-Kerner&amp;rft.au=William+Bialek&amp;rft.au=William+S.+Ryu&amp;bpr3.included=1&amp;bpr3.tags=Biology%2CMathematics%2CBioinformatics%2C+Applied+Mathematics%2C+Computational+Biology">Greg J. Stephens, Bethany Johnson-Kerner, William Bialek, William S. Ryu (2008). Dimensionality and Dynamics in the Behavior of C. elegans <span style="font-style:italic;">PLoS Computational Biology, 4</span> (4) DOI: <a rev="review" href="http://dx.doi.org/10.1371/journal.pcbi.1000028">10.1371/journal.pcbi.1000028</a></span></body></html>
