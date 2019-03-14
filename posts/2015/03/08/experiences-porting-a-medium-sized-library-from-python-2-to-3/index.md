<!--
.. title: Experiences porting a medium-sized library from Python 2 to 3
.. slug: experiences-porting-a-medium-sized-library-from-python-2-to-3
.. date: 2015-03-08 23:33:34
.. tags: Planet SciPy,programming,Python
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>Prompted in part by some discussions with Ed Schofield, creator of <a href="http://python-future.org">python-future.org</a>, I've been going on a bit of a porting spree to Python 3. I just finished with my gala segmentation library. (Find it on <a href="https://github.com/janelia-flyem/gala">GitHub</a> and <a href="http://gala.readthedocs.org">ReadTheDocs</a>.) Overall, the process is nowhere near as onerous as you might think it is. Getting started really is the hardest part. If you have more than yourself as a user, you should definitely just get on with it and port.

The second hardest part is the testing. In particular, you will need to be careful with dictionary iteration, pickled objects, and file persistence in general. I'll go through these gotchas in more detail below.

</p><h2>Reminder: the order of dictionary items is undefined</h2>

This is one of those <em>duh</em> things that I forget over and over and over. In my porting, some tests that depended on a scikit-learn <code>RandomForest</code> object were failing. I assumed that there was some difference between the random seeding in Python 2 and Python 3, leading to slightly different models between the two versions of the random forest.

This was a <em>massive</em> red herring that took me forever to figure out. In actuality, the seeding was completely fine. However, gala uses networkx as its graph backend, which itself uses an adjacency dictionary to store edges. So when I asked for <code>graph.edges()</code> to get a set of training examples, I was getting the edges in a <em>random order</em> that was <em>deterministic</em> within Python 2.7: the edges returned were always in the <em>same</em> shuffled order. This went out the window when switching to Python 3.4, with the training examples now in a <em>different</em> order, resulting in a <em>different</em> random forest and thus a <em>different</em> learning outcome... And finally a failed test.

The solution <em>should</em> have been to use a classifier that is not sensitive to ordering of the training data. However, although many classifiers satisfy this property, in practice they suffer from slight numerical instability which is sufficient to throw the test results off between shufflings of the training data.

So I've trained a Naive Bayes classifier in Python 2.7, and which I then load up in Python 3.4 and check whether the parameters are <em>close</em> to a newly trained one. The actual classification results can differ slightly, and this becomes much worse in gala, where classification tasks are sequential, so a single misstep can throw off everything that comes after it.

<h2>When pickling, remember to open files in binary mode</h2>

I've always felt that the pickle module was deficient for not accepting filenames as input to <code>dump</code>. Instead, it takes an open, writeable file. This is all well and good but it turns out that you should always <a href="http://www.diveintopython3.net/serializing.html">open files in binary mode when using pickle</a>! I got this far without knowing that, surely an indictment of pickle's API!

Additionally, you'll have specify a <code>encoding='bytes'</code> when loading a Python 2 saved file in the Python 3 version of pickle.

<h2>Even when you do, objects may not map cleanly between Python 2 and 3 (for some libraries)</h2>

In Python 2:

```
[code lang=python]
&gt;&gt;&gt; from sklearn.ensemble import RandomForestClassifier as RF
&gt;&gt;&gt; rf = RF()
&gt;&gt;&gt; from sklearn.datasets import load_iris
&gt;&gt;&gt; iris = load_iris()
&gt;&gt;&gt; rf = rf.fit(iris.data, iris.target)
&gt;&gt;&gt; with open('rf', 'wb') as fout:
...     pck.dump(r, fout, protocol=2)
[/code]
```

Then, in Python 3:

```
[code lang=python]
&gt;&gt;&gt; with open('rf', 'rb') as fin:
...     rf = pck.load(fin, encoding='bytes')
... 
---------------------------------------------------------------------------
KeyError                                  Traceback (most recent call last)
<ipython-input-9-674ee92b354d> in <module>()
      1 with open('rf', 'rb') as fin:
----&gt; 2     rf = pck.load(fin, encoding='bytes')
      3 

/Users/nuneziglesiasj/anaconda/envs/py3k/lib/python3.4/site-packages/sklearn/tree/_tree.so in sklearn.tree._tree.Tree.__setstate__ (sklearn/tree/_tree.c:18115)()

KeyError: 'node_count'
[/code]
```

<h2>When all is said and done, your code will probably run slower on Python 3</h2>

I have to admit: this just makes me angry. After <a href="https://github.com/janelia-flyem/gala/pull/39">a lot of hard work</a> ironing out all of the above kinks, gala's tests <a href="https://travis-ci.org/janelia-flyem/gala">run about 2x slower</a> in Python 3.4 than in 2.7. I'd heard quite a few times that Python 3 is slower than 2, but that's just ridiculous.

Nick Coghlan's <a href="http://python-notes.curiousefficiency.org/en/latest/python3/questions_and_answers.html">enormous Q&amp;A</a> has been <a href="http://sealedabstract.com/rants/python-3-is-fine/">cited</a> as required reading before complaining about Python 3. Well, I've read it (which took days), and I'm <em>still</em> angry that the CPython core development team are <a href="http://python-notes.curiousefficiency.org/en/latest/python3/questions_and_answers.html#why-doesn-t-this-limitation-really-bother-the-core-development-team">generally dismissive</a> of anyone wanting faster Python. Meanwhile, Google autocompletes "why is Python" with "so slow". And although Nick <a href="http://python-notes.curiousefficiency.org/en/latest/python3/questions_and_answers.html#what-about-insert-other-shiny-new-feature-here">asserts</a> that those of us complaining about this "misunderstand the perspective of conservative users", <a href="http://www.randalolson.com/2015/01/30/python-usage-survey-2014/">community surveys</a> show a whopping 40% of Python 2 users citing "no incentive" as the reason they don't switch.

<h2>In conclusion...</h2>

In the end, I'm glad I ported my code. I learned a few things, and I feel like a better Python "citizen" for having done it. But that's the point: those are pretty weak reasons. Most people just want to get their work done and move on. Why would they bother porting their code if it's not going to help them do that?</module></ipython-input-9-674ee92b354d></body></html>
