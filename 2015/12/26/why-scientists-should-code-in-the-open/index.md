<!--
.. title: Why scientists should code in the open
.. slug: why-scientists-should-code-in-the-open
.. date: 2015-12-26 16:17:05
.. tags: open access,open-source,Planet SciPy,programming
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>All too often, I encounter <em>published papers</em> in which the code is "available upon request", or "available in the supplementary materials" (as a zip file). This is not just poor form. It also hurts your software's future. (And, in my opinion, when results depend on software, it is inexcusable.)

Given the numerous options for posting code online, there's just no excuse to give code in a less-than-convenient format, upon publication. When you publish, put your code on <a href="https://github.com">Github</a> or <a href="https://bitbucket.com">Bitbucket</a>.

In this piece, I'll go even further: put your code there <em>from the beginning</em>. Put your code there <em>as soon as you finish reading this article</em>. Here's why:

</p><h2>No, you won't get scooped</h2>

Reading code is <em>hard</em>. Ask any experienced programmer: most have trouble reading code <em>they themselves wrote</em> a few months ago, let alone someone else's code. It's extremely unlikely that someone will browse your code looking for a scoop. That time is better spent doing research.

<h2>It's never going to be ready</h2>

Another thing I hear is that they <em>want</em> to post their code, but they want to clean it up first, and remove all the "embarrassing" bits. Unfortunately, science doesn't reward time spent "cleaning up" your code, at least not yet. So the sad reality is that you probably will never actually get to the point where you are happy to post your code online.

But here's the secret: <em>everybody is in that boat with you</em>. That's why <a href="http://matt.might.net/articles/crapl/">this document</a> exists. I recommend you read it in full, but this segment is particularly important:

<blockquote>
  When it comes time to empirically evaluate new research with respect to someone else's prior work, even a rickety implementation of their work can save grad-student-months, or even grad-student-years, of time.
</blockquote>

Matt Might himself is as thorough and high-profile as you get in computer science, and yet, he has this to say about code clean-up:

<blockquote>
  I kept telling myself that I'd clean it all up and release it some day.
  
  I have to be honest with myself: this clean-up is never going to happen.
</blockquote>

Your code might not meet your standards, but, believe it or not, your code <em>will</em> help others, and the sooner it's out there, the sooner they can be helped.

<h2>You will gain collaborators and citations</h2>

If anyone is going to be rifling through your code, they will probably end up asking for your help. This happens with even the best projects: have a look at the activity on the mailing lists for <a href="http://sourceforge.net/p/scikit-learn/mailman/scikit-learn-general/">scikit-learn</a> or <a href="http://www.mail-archive.com/numpy-discussion@scipy.org/">NumPy</a>, two of the best-maintained open-source projects out there.

When you have to go back and explain how a piece of code worked, <em>that's</em> when you will actually take the time and clean it up. In the process, the person you help will be more likely to contribute to your project, either in code or in bug reports, improvement suggestions, or even citations.

In the case of my own <a href="https://github.com/janelia-flyem/gala">gala</a> project, I guess that about half of the citations it received happened because of its open-source code and open <a href="http://gala.30861.n7.nabble.com">mailing list</a>.

<h2>Your coding ability will automagically improve</h2>

I first heard this one from <a href="http://albert.rierol.net">Albert Cardona</a>. They say sunlight is the best disinfectant, and this is certainly true of code. Just the very idea that anyone can easily read their code will make most people more careful when programming. Over time, this care will become second nature, and you will develop a taste for nice, easy-to-read code.

In short, the alleged downsides of code-sharing are, at best, longshots, while there are many tangible upsides. Put your code out there. (And use a <a href="http://www.astrobetter.com/blog/2014/03/10/the-whys-and-hows-of-licensing-scientific-code/">liberal open-source license</a>!)</body></html>
