<!--
.. title: Why you should cite open source tools
.. slug: why-you-should-cite-open-source-tools
.. date: 2019-05-02 12:31:30 UTC+10:00
.. tags: Planet SciPy,open-source,science,programming,Python
.. category: 
.. link: 
.. description: 
.. type: text
-->

Every now and then, a moment or a sentence in a conversation sticks out at you,
and lodges itself in the back of your brain for months or even years. In this
case, the sentence is a tweet, and I fear that the only way to dislodge it is
to talk about it publicly.

Last year, I complained on Twitter that a very prominent paper that was getting
lots of attention used scikit-image, but failed to cite our paper. (Or the
papers corresponding to many other open source packages.) I continued that
scientists developing open source software *depend* on these citations to
continue their work. (More on this in another post...) One response was that
surely the developers of the open source scientific Python stack were not
scientists per se, and that citations were not a priority for them.

I still sigh internally when I think of it.

That tweet manifests a pervasive perception that open source scientific
software is written by God-like figures. These massively experienced software
developers have easy access to funds for their work, and are at the service of
all the other scientists, who are their users. I used to share this perception,
but it is utterly false.

I certainly hadn't thought about the funding question, but I did think of
packages like NumPy and SciPy as being written by "pros" (whatever that means),
whose main job was to produce these amazing libraries.

My ideas started to change only after I attended the SciPy 2012 conference, and
I was invited to the scikit-image sprint by its lead author, [Stéfan van der
Walt](https://mentat.za.net/). I was totally starstruck, and even after meeting
him I just assumed his job was "open source guru", or somesuch. It is only
later that I learned that he was a postdoctoral researcher and lecturer in
applied mathematics at Stellenbosch University, in South Africa. As with other
academics, his main job was to produce research and to teach. Open source was
something he produced on the side.

(Years later, as we were scrambling to finish the final chapter for
[Elegant SciPy](http://elegant-scipy.org), Stéfan revamped the build
infrastructure for our book — the scripts and configuration files that
converted the Markdown text we were writing to executed code and html. "You
have poor prioritisation skills, you know?", I taunted. He responded: "Yeah.
But a lot of the SciPy documentation toolchain exists because of that." And
yes: the revamped scripts ended up being extremely useful during the editing
phase with O'Reilly.)

After the conference I continued to contribute to scikit-image, eventually
joining the core development team, but throughout the process I continued to
feel like an [impostor](https://en.wikipedia.org/wiki/Impostor_syndrome),
someone who had somehow managed to gain entrance to this hallowed and
otherworldly community despite his inferior skills and knowledge.

Only after years of interacting with this community did I internalise the fact
that nearly all of this software stack has been produced by practising
scientists who took the extra care and effort to ensure that their code was
robust, well-tested, and easily accessible to all. Despite a recent influx of
interest and contributions from industry, as far as I can tell, most
contributions to the SciPy stack still come from practising scientists in
academia.

I know this now, but until recently I was suffering from another fallacy: that
if I knew it, clueless as I was, then surely everybody knew it. That tweet
disabused me of that notion, and this is why you're reading this. I hope it is
instructive, and that you'll find it worth sharing widely.

This idea, that the SciPy stack is made by active scientists, is important
because it affects how this work can be supported. Sadly, neither university
hierarchies nor national funding bodies recognise code as valuable output.
(There are some
[exceptions](https://twitter.com/ethanwhite/status/1083008449242378240), but
this remains the norm.) By and large, the only things that count are papers,
grants, and, to a lesser extent, teaching evaluation scores. 

So, yes, many of the original contributors to the SciPy libraries are now in
industry. But they were academics at the time that they contributed and were
driven out because academia did not value their contributions. Academics should
not have to sacrifice their careers to contribute to open source. Citations to
software papers are an imperfect solution to this problem (again, more soon),
but they sure as hell are better than nothing.

So, if you are a user of open source tools in the Scientific Python stack, I
have two requests for you:

1. When you publish your work, cite every library that you import. Most
   scientific software has a notice on their homepage or README file pointing
   to a paper you can cite. By definition, if you've imported a library, you've
   found it useful, and if you've found it useful, then you probably care about
   supporting its authors. This is a small way you can contribute to their
   success.
2. You are good enough to contribute. If you have an issue
   with an open source package you are using, look at the source code. Submit
   an issue to the project's bug tracker (usually GitHub). And try your hand at
   fixing it. The software's authors will usually offer guidance on
   how to do this, and you will improve your own skills as a result. Good
   software development practices is one of the most transferrable skills you
   can gain.

Of course, citations alone will not solve the wider problem that open source
software is chronically undervalued. I have many thoughts about how open source
should be supported, especially in science, but I'll expand on that in an
upcoming post.

**Update:** I'm adding a link to a related post: [What do scientists know about
open source?](http://ilovesymposia.com/2018/06/20/what-do-scientists-know-about-open-source/)
