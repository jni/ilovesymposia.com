<!--
.. title: Clarifications about our book, Elegant SciPy (and our call for code submissions)
.. slug: clarifications-about-our-book-elegant-scipy-and-our-call-for-code-submissions
.. date: 2015-02-23 06:31:46
.. tags: Planet SciPy,Python,Scientific Computing,programming
.. category: 
.. link: 
.. description: 
.. type: text
.. excerpt: A big thank you to everyone who has already submitted, retweeted, and spread the word about [our book, Elegant SciPy](http://ilovesymposia.com/2015/02/04/call-for-code-nominations-for-elegant-scipy/)! We are **still looking for code submissions meeting these criteria**:
- Submissions **must** use NumPy, SciPy, or a closely related library in a non-trivial way.
- Submissions **must** be (re)licensed as BSD, MIT, public domain, or something similarly liberal. (This is easy if you are the author.)
- Code should be satisfying in some way. ;) e.g. speed, conciseness, broad applicability...
- Preferably, nominate *someone else's* code that impressed *you*.
- Include a scientific application on real data.
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><h2>Short version</h2>

Thank you to everyone who has already submitted, retweeted, and spread the word about <a href="http://ilovesymposia.com/2015/02/04/call-for-code-nominations-for-elegant-scipy/">our book, Elegant SciPy</a>! We are <strong>still looking for code submissions meeting these criteria</strong>:
- Submissions <strong>must</strong> use NumPy, SciPy, or a closely related library in a non-trivial way.
- Submissions <strong>must</strong> be (re)licensed as BSD, MIT, public domain, or something similarly liberal. (This is easy if you are the author.)
- Code should be satisfying in some way, such as speed, conciseness, broad applicability...
- Preferably, nominate <em>someone else's</em> code that impressed <em>you</em>.
- Include a scientific application on real data.

Submit by one of:
- Twitter: mention <a href="https://twitter.com/hdashnow">@hdashnow</a>, <a href="https://twitter.com/stefanvdwalt">@stefanvdwalt</a>, or <a href="https://twitter.com/jnuneziglesias">@jnuneziglesias</a>, or just use the hashtag #ElegantSciPy;
- Email: <a href="mailto:stefanv(at)berkeley.edu">Stéfan van der Walt</a>, <a href="mailto:juan.n@unimelb.edu.au">Juan Nunez-Iglesias</a>, or <a href="mailto:harriet.dashnow@unimelb.edu.au">Harriet Dashnow</a>; or
- GitHub: create a new issue <a href="https://github.com/HarrietInc/elegant-scipy-submissions/issues">here</a>.
(All submissions will be credited as "written by" and "nominated by".)

<h2>Long version</h2>

A big thank you to everyone that has submitted code, retweeted, posted on mailing lists, and otherwise spread the word about the book! We're still pretty far from a book-length title though. I was also a bit vague about the kinds of submissions we wanted. I'll elaborate a bit on each of the above points:

<h3>NumPy and SciPy use</h3>

Some excellent submissions did not use the SciPy library, but rather did amazing things with the Python standard library. I should have mentioned that the book will be specifically focused on the NumPy and SciPy libraries. That's just the scope of the book that O'Reilly has contracted us to write. Therefore, although we might try to fit examples of great Python uses into a chapter, they are not suitable to be the centerpieces.

We will make some exceptions, for example for very closely related libraries such as pandas and scikit-learn. But, generally, the scope is SciPy the library.

<h3>Licensing</h3>

This one's pretty obvious. We can't use your submission if it's under a restrictive license. And we don't want to publish GPL-licensed code, which could force our readers to GPL-license their own code when using it. For more on this, see <a href="http://choosealicense.com">Choose A License</a>, as well as Jake Vanderplas's excellent <a href="http://www.astrobetter.com/the-whys-and-hows-of-licensing-scientific-code/">blog post</a> encouraging the use of the BSD license for scientific code.

Note that the author of a piece of code is free to relicense as they choose, so if you think some GPL code is perfect and might be amenable to relicensing, do let us know about it!

<h3>Submitting someone else's code</h3>

I suspect that a lot of people are shy about submitting their own code. Two things should alleviate this. First, you can now submit via email, so you don't have to be public about the self-promotion. (Not that there's anything wrong with that, but I know I sometimes struggle with it.) And second, I want to explicitly state that we <em>prefer it</em> if you submit <em>others'</em> code. This is not to discourage self-promotion, but to drive code quality even higher. It's a high bar to convince ourselves that our code is worthy of being called elegant, but it takes another level entirely to find someone else's code elegant! (The usual reaction when reading other people's code is more like, "and what the <em>#$&amp;^</em> is going on <em>here????</em>") So, try to think about times you saw great code during a code review, reading a blog post, or while grokking and fiddling with someone else's library.

<h3>Data</h3>

Beautiful code is kind of a goal unto itself, but we really want to demonstrate how <em>useful</em> SciPy is in real scientific analysis. Therefore, although cute examples on synthetic data can illustrate quite well what a piece of code does, it would be extremely useful for us if you can point to examples with real scientific data behind them.

Thank you, and we hope to hear from you!</body></html>
