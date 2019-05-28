<!--
.. title: EuroSciPy 2015 debrief
.. slug: euroscipy-2015-debrief
.. date: 2015-10-09 01:06:01
.. tags: Planet SciPy,Python,SciPy,conference,programming
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>The videos from EuroSciPy 2015 are <a href="https://www.youtube.com/playlist?list=PLYx7XA2nY5GeQCCugyvtnHMVLdhYlrRxH">up</a>! This marks a good time to write up my thoughts on the conference.
I’ve <a href="http://ilovesymposia.com/2015/03/23/go-to-scipy-2015/">mentioned before</a> that the yearly SciPy conference is stunningly useful. This year I couldn’t make it to Austin, but I did attend <a href="https://www.euroscipy.org/2015/">EuroSciPy</a>, the European version of the same conference, in Cambridge, UK. It was spectacular.

<!-- TEASER_END -->

</p><h2>Useful talks</h2>

The talk of the conference, for me, goes to <a href="https://youtu.be/8tysix6Olgc">Robin Wilson for recipy</a>, which one can describe as a logging utility, if one wishes to make it sound as uninspiring as possible. Recipy’s strength is in its mind-boggling simplicity. Here is the unabridged usage guide:

```python
import recipy
```

With this single line, your script will now generate an entry in a database every time it is run. It logs the start and end time, the working directory, the script's git hash, any differences between the working copy and the last git commit (!), and the names of any input and output files. (File hashes are coming soon, <a href="https://twitter.com/sciremotesense/status/646951148608425984">I’m assured</a>).
I don’t know about you but I have definitely lost count of the times I’ve looked at a file and wondered what script I ran to get it, or the input data that went into it. This library solves that problem with absolutely minimal friction for the user.
I also enjoyed Nicolas Rougier’s <a href="https://youtu.be/orLYVhSrOwk">talk</a> on <a href="https://rescience.github.io">ReScience</a>, a new journal dedicated to replicated (and replicable) scientific analyses. It’s a venue to publish all those efforts to replicate a result you read in a paper. Given recent findings about how <em>poorly</em> most papers replicate, I think this is a really important outlet.
The other remarkable thing about it is that all review is open and done in the spirit of open source, on GitHub. Submission is by <a href="https://help.github.com/articles/using-pull-requests/">pull request</a>, of course. With just one paper out so far, it’s a bit early to tell whether it’ll take off, but I really hope it does. I’ll be looking for stuff of my own to publish there, for sure. (Oh and by the way, they are <a href="http://rescience.github.io/faq/">looking for reviewers and editors!</a>)
Another great <a href="https://youtu.be/_vMq8Z82poA">talk</a> was Philipp Rudiger on <a href="http://holoviews.org">HoloViews</a>, an object-oriented plotting framework. They define an arithmetic on figures: <code>A * B</code> overlays figure B on A, while <code>B + C</code> creates two subplots out of B and C (and automatically labels them). Their <a href="http://holoviews.org/Examples/index.html">example notebooks</a> rely a lot on IPython magic, which I’m not happy about and means I haven’t fully grokked the API, but it seems like a genuinely useful way to think about plotting.
A final highlight from the main session was <a href="https://youtu.be/MeFsmFTU2JQ">Martin Weigert on Spimagine</a>, his GPU-accelerated, 5D image analysis and visualisation framework. It was stupidly impressive. Although it’s a long-term project, I’m inclined to try to incorporate many of its components into <a href="http://scikit-image.org">scikit-image</a>.

<h2>Tutorials</h2>

The tutorials are a great asset of both EuroSciPy and SciPy. I learn something new every year. The highlight for me was the <a href="https://youtu.be/GmxZfZjEjZo">Cython tutorial</a>, in which Stefan Behnel demonstrated how easy it is to provide Python access to C++ code using Cython. (I have used Cython quite extensively, but only to speed up Python code, rather than wrap C or C++ code.)

<h2>Sprints</h2>

I was feeling a bit hypocritical for missing the sprints this year, since I had to run off before the Sunday. Emmanuelle Gouillart, another scikit-image core dev, suggested having a small, unofficial sprint on Friday evening. It grew and grew into a group of about 30 people (including about 10 new to sprinting) who all gathered at the Enthought Cambridge office to work on scikit-image or the <a href="http://www.scipy-lectures.org">SciPy lecture notes</a>. A brilliant experience.
<img src="https://pbs.twimg.com/media/CNg3PuJWgAAj0Qq.jpg" alt="scikit-image sprint at Enthought">
(By the way, nothing creepy going on with that dude hunching over one of our sprinters — that's just husband-and-wife team Olivia and Robin Wilson! ;)

<h2>Final thoughts</h2>

As usual, I learned heaps and had a blast at this SciPy conference (my fourth). I hope it will remain a yearly ritual, and I hope someone reading this will give it a try next year!</body></html>
