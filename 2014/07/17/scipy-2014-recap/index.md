<!--
.. title: SciPy 2014: an extremely useful conference with a diversity problem
.. slug: scipy-2014-recap
.. date: 2014-07-17 18:48:56
.. tags: diversity,Planet SciPy,programming,women in science,conference
.. category: 
.. link: 
.. description: 
.. type: text
.. status: published
-->

<html><body><p>I just got back home from the <a href="https://conference.scipy.org/scipy2014/">SciPy 2014</a> conference in Austin, TX. Here are my thoughts after this year's conference.

</p><h2>About SciPy in general</h2>

Since my first SciPy in 2012, I've decided to prioritise programming conferences over scientific ones, because the value I get for my time is just that much higher. At a scientific conference, I certainly become aware of way-cool stuff going on in other research labs in my area. Once I get home, however, I go back to whatever I was doing. In contrast, at programming conferences, I become aware of new tools and practices that <em>change the way I do science.</em> In his <a href="https://www.youtube.com/watch?v=1e26rp6qPbA">keynote</a> this year, Greg Wilson said of Software Carpentry, "We save researchers a day a week for the rest of their careers." I feel the same way about SciPy in general.

In the 2012 sprints, I learned about <a href="https://help.github.com/articles/using-pull-requests">GitHub Pull Requests</a> and code review, the lingua franca of open source development today. I can't express how useful that's been. I also started my ongoing collaboration with the <a href="http://scikit-image.org/">scikit-image</a> project, which has enabled my research to reach far more users than I ever could have achieved on my own.

No scientific conference I've been to has had such an impact on my science output, nor can I imagine one doing so.

<!-- TEASER_END -->

<h2>This year's highlights</h2>

This year was no different. Without further ado, here are my top hits from this year's conference:

<ul>
<li>Michael Droettboom <a href="https://www.youtube.com/watch?v=OsxJ5O6h8s0">talked</a> about his continuous benchmarking project, <a href="https://spacetelescope.github.io/asv/">Airspeed Velocity</a>. It is hilariously named and incredibly useful. <code>asv</code> checks out code from your Git repo at regular intervals and runs benchmarks (which you define), and plots your code's performance over time. It's an incredible guard against feature creep slowing down your code base.</li>
<li>IPython recently unveiled their modal version 2.0 interface, sending vimmers worldwide rejoicing. But a few key bindings are just <em>wrong</em> from a vim perspective. Most egregiously, <code>i</code>, which should enter edit mode, interrupts the kernel when pressed twice! That's just evil. Paul Ivanov goes all in with vim keybindings with his hilarious and life-changing <a href="https://www.youtube.com/watch?v=p9gnhmX1sPo">IPython vimception</a> talk. The title is more appropriate than I realised. Must-watch.</li>
<li>Damián Avila <a href="https://www.youtube.com/watch?v=sZBKruEh0jI">revealed</a> (heh) his live IPython presentations with Reveal.js, forever changing how Python programmers present their work. I was actually aware of this before the conference and used it in <a href="https://www.youtube.com/watch?v=fn8F_NerTug">my own talk</a>, but you should definitely watch his talk and get the extension from his <a href="https://github.com/damianavila/live_reveal">repo</a>.</li>
<li>Min RK gave an excellent tutorial on IPython parallel (<a href="https://github.com/minrk/IPython-parallel-tutorial">repo</a>, videos <a href="https://www.youtube.com/watch?v=y4hgalfhc1Y">1</a>, <a href="https://www.youtube.com/watch?v=-9ijnHPCYhY">2</a>, <a href="https://www.youtube.com/watch?v=U5mhpKkIx2Y">3</a>). It's remarkable what the IPython team have achieved thanks to their decoupling of the interactive shell and the computation "kernel". (I still hate that word.)</li>
<li>Brian Granger and Jon Frederic gave an excellent <a href="https://www.youtube.com/watch?v=aIXED26Wppg">tutorial</a> on IPython interactive widgets (notebook <a href="https://github.com/ipython/ipython-in-depth">here</a>). They provide a simple and fast way to interactively explore your data. I've already started using these on my own problems.</li>
<li>Aaron Meurer gave the <a href="https://www.youtube.com/watch?v=UaIvrDWrIWM">best introduction</a> to the Python packaging problem that I've ever seen, and how Continuum's conda project solves it. I think we still need an in-depth tutorial on how package <em>distributors</em> should use conda, but for users, conda is just awesome, and this talk explains why.</li>
<li>Matt Rocklin has a gift for crystal clear speaking, despite his incredible speed, and it was on full display in his (and Mark Wiebe's) talk on <a href="https://www.youtube.com/watch?v=9HPR-1PdZUk">Blaze</a>, Continuum's new array abstraction library. I'm not sure I'll be using Blaze immediately but it's certainly on my radar now!</li>
<li>Lightning talks are always fun: days <a href="https://www.youtube.com/watch?v=JDrhn0-r9Eg">1</a>, <a href="https://www.youtube.com/watch?v=SMyto7WHiNs">2</a>, <a href="https://www.youtube.com/watch?v=ln4nE_EVDCg">3</a>. Watch out for Fernando Pérez's announcement of Project Jupyter, the evolution of the IPython notebook, and for Damon McDougall's riveting history of waffles. (You think I'm joking.)</li>
</ul>

Apologies if I've missed anyone: with three tracks, an added one with the World Cup matches ;) , and my own talk preparations, "overwhelming" does not begin to describe the conference! I will second Aaron Meurer's <a href="https://asmeurer.github.io/blog/posts/scipy-2014/">assertion</a> that there were no bad talks. Which brings us to...

<h2>On my to-watch</h2>

Jake Vanderplas recently wrote a series of blog posts (last one <a href="http://jakevdp.github.io/blog/2014/06/14/frequentism-and-bayesianism-4-bayesian-in-python/">here</a>, with links to earlier posts) comparing frequentist and Bayesian approaches to statistical analysis, in which he makes a compelling argument that we should all relegate frequentism to the dustbin of history. As such, I intend to go over Chris Fonnesbeck's <a href="https://www.youtube.com/watch?v=vOBB_ycQ0RA">tutorial</a> (<a href="https://www.youtube.com/watch?v=gFYPCdWB2-w">2</a>, <a href="https://www.youtube.com/watch?v=54sFjp7AvXM">3</a>) and <a href="https://www.youtube.com/watch?v=XbxIo7ScVzc">talk</a> about Bayesian analysis in Python using PyMC.

David Sanders also did a Julia <a href="https://www.youtube.com/watch?v=vWkgEddb4-A">tutorial</a> (<a href="https://www.youtube.com/watch?v=I3JH5Bg46yU">part 2</a>) that was scheduled at the same time as my own scikit-image tutorial, but I'm interested to see how the Julia ecosystem is progressing, so that should be a good place to start. (Although I'm still bitter that they went with 1-based indexing!)

The <a href="https://www.youtube.com/watch?v=EzX7MN_bzqg">reproducible science tutorial</a> (<a href="https://www.youtube.com/watch?v=HCyHn_by3N0">part 2</a>) generated quite a bit of buzz so I would be interested to go over that one as well.

For those interested in computing education or in geoscience, the conference had dedicated tracks for each of those, so you are bound to find something you like (not least, Lorena Barba's and Greg Wilson's keynotes). Have a look at the full listing of videos <a href="https://www.youtube.com/playlist?list=PLYx7XA2nY5GfuhCvStxgbynFNrxr3VFog">here</a>. These might be easier to navigate by looking at the <a href="https://conference.scipy.org/scipy2014/schedule/">conference schedule</a>.

<h2>The SciPy experience</h2>

I want to close this post with a few reflections on the conference itself.

SciPy is broken up into three "stages": two days of tutorials, three days of talks, and two days of sprints. Above, I covered the tutorials and talks, but to me, the sprints are what truly distinguish SciPy. The <a href="https://twitter.com/ellisonbg/status/487731404675899392">spirit of collaboration</a> is unparalleled, and an astonishing amount of value is generated in those two days, either in the shape of code, or in introducing newcomers to new projects and new ways to collaborate in programming.

My biggest regret of the conference was not giving a lightning talk urging people to come to the sprints. I repeatedly asked people whether they were coming to the sprints, and almost invariably the answer was that they didn't feel they were good enough to contribute. To reiterate my previous statements: (1) when I attended my first sprint in 2012, <em>I had never done a pull request</em>; (2) sprints are an excellent way to introduce newcomers to projects and to the pull request development model. All the buzz around the sprints was how welcoming all of the teams were, but I think there is a massive number of missed opportunities because this is not made obvious to attendees <em>before</em> the sprints.

Lastly, a few notes on diversity. During the conference, April Wright, a student in evolutionary biology at UT Austin, wrote a heartbreaking <a href="https://wrightaprilm.github.io/posts/lonely.html">blog post</a> about how excluded she felt from a conference where only 15% of attendees were women. That particular incident was joyfully resolved, with plenty of SciPyers reaching out to April and inviting her along to sprints and other events. But it highlighted just how poorly we are doing in terms of diversity. Andy Terrel, one of the conference organisers, <a href="https://twitter.com/aterrel/status/487281782530650112">pointed out</a> that 15% is much better than 2012's three (women, not percent!), but (a) that is still extremely low, and (b) I was horrified to read this because I was there in 2012... <em>And I did not notice that anything was wrong.</em> How can it be, in 2012, that it can seem <em>normal</em> to be at a professional conference and have effectively zero women around? It doesn't matter what one says about the background percentage of women in our industry and so on... Maybe SciPy is doing all it can about diversity. (Though I doubt it.) The point is that a scene like that should <em>feel</em> like one of those deserted cityscapes in post-apocalyptic movies. As long as it doesn't, as long as SciPy feels normal, we will continue to have diversity problems. I hope my fellow SciPyers look at these numbers, feel appalled as I have, and try to improve.

... And on cue, while I was writing this post, Andy Terrel wrote a great post of his own about this very topic:
http://andy.terrel.us/blog/2014/07/17/

I still consider SciPy a fantastic conference. Jonathan Eisen (<a href="https://twitter.com/phylogenomics">@phylogenomics</a>), whom I admire, would undoubtedly boycott it because of the problems outlined above, but I am heartened that the organising committee is taking this as a serious problem and trying hard fix it. I hope next time it is even better.</body></html>
