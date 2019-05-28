<!--
.. title: Why citations are not enough for open source software
.. slug: why-citations-are-not-enough-for-open-source-software
.. date: 2019-05-28 18:41:54 UTC+10:00
.. tags: Planet SciPy,open-source,science,programming,Python
.. category: 
.. link: 
.. description: 
.. type: text
-->

A few weeks ago I wrote about [why you should cite open source
tools](https://ilovesymposia.com/2019/05/02/why-you-should-cite-open-source-tools/).
Although I think citations important, though, there are major problems in
relying on them *alone* to support open source work.

The biggest problem is that papers describing a software library can only give
credit to the contributors at the time that the paper was written. The
preferred citation for the SciPy library is “Eric Jones, Travis Oliphant, Pearu
Peterson, *et al*”, 2001. The “*et al*” is not an abbreviation here, but a
fixed shorthand for all other contributors. Needless to say many, *many* people
have contributed to the SciPy library since 2001 (GitHub counts 716
contributors as of this writing), and they are unable to get credit within the
academic system for those contributions. (As an aside, Google counts about
1,200 citations to SciPy, which is a breathtaking undercounting of its value
and influence, and reinforces my earlier point: **cite open source software!
Definitely don't use this post as an excuse not to cite it!!!**)

Not surprisingly, we have had massive contributions to scikit-image since our
2014 paper, and those contributors miss out on the citations to our paper.

<!-- TEASER_END -->

From a maintainer point of view, one way to deal with this is to periodically
write “update” publications that become the preferred way to cite the
software. This is the approach taken by the
[CellProfiler](https://cellprofiler.org/citations/) team, for example, and the
one we will take for scikit-image. This allows new contributors to benefit,
while also preventing infinite returns for the original authors, which is as
it should be.

I think this is as good as project maintainers can do now, but leaves open the
questions of authorship, publication frequency, and the potential dilution of
the authors' message.

A *proper* fix requires structural change that directly recognises the value of
open source software to science. Software papers are in fact a *hack* of the
academic system, a currency conversion, and as with any conversion, there are
losses and inefficiencies involved. Indeed, the paper provides less value than
good documentation for the software. (Last year, when someone tweeted about their
latest software paper, many replies asked for the GitHub link in the
abstract! People wanted to look at the software, not the paper.)

So what does open source software *really* need, if not citations? Of course,
it's money. Citations are useful because they can *potentially* be translated
into grants, promotions, and jobs, but wouldn't it be great if we could cut out
the middleman and just have great open source translate *directly* into
grants, promotions, and jobs?

The insane thing is that granting bodies actually give enormous amounts of
money to *closed source* software developers, since grant money is routinely
spent on software licenses. Instead of subsidising proprietary software, that
money could be used to fund open source developers, and improve software for
*everyone*.

## How to fund open source

I might have left the how for a later post, but while I was drafting this, CZI
[announced](https://twitter.com/cziscience/status/1128693937130991623) direct
funding specifically for open source software. Needless to say I think this is
wonderful. I am funded by an earlier closed
[funding round](https://www.chanzuckerberg.com/newsroom/czi-announces-support-for-open-source-software-efforts-to-improve-biomedical-imaging)
to develop scikit-image, and I thought a lot at the time about all the other
deserving, unfunded projects that I use. An open call is absolutely the right
thing to do, and supporting *maintenance* rather than development of hot new
things, as this CZI call is doing, is also the right thing to do. (Others have
flagged that a single year of support, even if renewable, makes it difficult to
support careers, and I agree, but this is a new space, and it makes sense that
CZI would dip its toes before jmuping in.)

The response to CZI's announcement has been absolute fire. There is enormous
pent-up need and
[frustration](https://twitter.com/amuellerml/status/1117455802598662144) over
funding
[maintenance and improvement](https://twitter.com/story645/status/1117567608222564353)
for open source tools
[underpinning](https://twitter.com/jnuneziglesias/status/1131080022750519296)
an enormous amount of science. (Even a month later, reading Andreas's tweet
makes me want to scream in frustration in the crowded train in which I write
this. Not impactful enough!?) Given the buzz around CZI's call, I wonder
whether CZI has enough staff to even read through the avalanche of applications
it will receive. My hope is that national funding bodies will take notice and
start funding open source maintenance work, as Germany has
[recently done](https://www.dfg.de/en/research_funding/programmes/infrastructure/lis/funding_opportunities/call_proposal_software/).
(It's unclear to me whether Germany has repeated that call since 2016. If you
know, please leave a comment below!)

Since I'm an academic, for a long time I was stuck in the mindset that granting
bodies need to step up and recognise that *established* open source is a
valuable thing to fund, and this is certainly true. More recently though, as I
dipped my toes into some private sector consulting, it occurred to me that the
situation is completely ridiculous — “Hey look at this incredible thing we
built, it's serving thousands to millions of scientists, can we get some money
to keep it going?” “It's done and it's free, why would we give you money?” It's
a fundamental misunderstanding at the very top of funding agencies of how
software maintenance works.

There are plenty of people that *do* understand the value of the software,
though: its users, many of whom are in a position to use grant money to
help maintain it. But open source developers have not thus far made it
easy to ~~donate money to~~ invest in their projects. I'm not talking about a
little donate button on the homepage, which will never substantially support
a project. Rather, open source projects should *sell* a product, be it a
maintenance contract, a support contract, or a new feature development, or even
a *feel-good* contract. But “buying” an open source software package should
feel exactly like buying closed-source software, and to University purchasing
departments it should look exactly like a normal software purchase.

As I was thinking about this idea, I moved my blog from wordpress.com, for
which I was paying $100 per year, to [Nikola](https://getnikola.com/), an
open source static site generator written in Python. I *love* Nikola, and was
ready to put my $100 towards the project, but to my surprise, even a donate
button was absent. As Andreas Mueller and Chris Holdgraf have
[pointed out](https://twitter.com/choldgraf/status/1070672075692613636),
many projects haven't even *considered* what they would do with a large influx
of money if they got one. That needs to change.

Thankfully, organizations including
[NumFOCUS](https://mail.python.org/pipermail/numpy-discussion/2019-April/079326.html),
[QuanSight](https://www.quansight.com/open-source-support),
[Tidelift](https://tidelift.com/), and others are working hard to rectify
this situation. I'm trying to help where I can and I really look forward to
seeing what they come up with.

If you are interested in this topic, we are
[organising](https://github.com/numfocus/scipy-2019-funding-foss-bof) a Birds
of a Feather session to take place at the
[SciPy 2019](https://www.scipy2019.scipy.org/) in Austin in July.
Please join us!
