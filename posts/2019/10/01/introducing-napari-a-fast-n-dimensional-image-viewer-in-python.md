<!--
.. title: Introducing napari: a fast n-dimensional image viewer in Python
.. slug: introducing-napari-a-fast-n-dimensional-image-viewer-in-python
.. date: 2019-10-01 23:07:54 UTC+10:00
.. status: draft
.. tags: 
.. category: 
.. link: 
.. description: 
.. type: text
-->

I'm really excited to finally share a new(ish) project, napari, with the world.
We have been developing napari [in the open](https://github.com/napari/napari)
from the very first commit, but we didn't want to make any premature fanfare
about it… Until now. It's still alpha software, but for months now, both the
core napari team and a few collaborators/early adopters have been using napari
in our daily work. I've found it *life-changing*.

# The background

I've been looking for a great nD volume viewer in Python for the better part of
a decade. I started working on the segmentation of 3D electron microscopy (EM)
volumes at the Janelia [Farm] Research Campus in 2009. I started out in Matlab,
but moved to Python pretty quickly and it was a very smooth transition (highly
recommended! ;). Looking at my data was always annoying though. I was either
looking at single 2D slices using `matplotlib.pyplot.imshow`, or saving the
volumes in VTK format and loading them into ITK-SNAP — which worked ok but
required me to constantly be changing terminals, and anyway was pretty
inefficient because my disk ended up cluttered with “temporary” VTK volumes.

I tried a bunch of stuff since then, including Mayavi and even building my own
orthogonal views viewer with Matplotlib, but nothing really clicked. What's
worse, I didn't see any movement on this — I even found a very discouraging
thread on MNE-Python in which Gaël Varoquaux concluded, given the lack of
support, that 3D visualisation was not mission critical to anyone and not worth
pursuing further. (!) Throughout that time, I kept living with either manually
picking out 2D slices, or going back to tools outside of Python, with all the
context switching that that entailed. The end result is that I was looking at
my data far less than I should have been, which slowed down my work. I can't
count the number of silly mistakes I made and dead ends I chased, just because
I wasn't looking at the data enough.

# The beginning

In the meantime though, I was busy improving support for n-dimensional images
in scikit-image. In May last year, Nelle Varoquaux organised joint sprint with
developers from scikit-learn, scikit-image, and dask at UC Berkeley, and I was
one of the lucky invitees. I'd recently seen that Loïc Royer, a good friend
from my Janelia days, had moved to San Francisco to start his lab at the Chan
Zuckerberg Biohub, and I asked him if I could crash at his place for a few days
before the sprint.

I knew Loïc as a Java guy. In Janelia he'd repeatedly extolled the virtues of
Java to me, and since then
he'd worked on some incredible Java software, including ClearVolume, a super-fast
3D viewer, and ClearControl, a super-fast microscope control library (you
might detect a theme there ;).
In fact, although I love Python and its ecosystem and what we've built with
scikit-image, I've always been intimidated by Fiji and its ecosystem
and its ~~10,000~~ 15,000 citations. Was I deluding myself that Python was
important in biology?

So I was both shocked and delighted when Loïc casually mentioned that he needed
a fast nD viewer in Python, and that we should build it together. Loïc had been
pushing the state of the art of deep learning in microscopy (and continues to
do so), and, despite the hard work of many to integrate Fiji with the major deep learning
learning libraries (which are Python), as well as the wider scientific Python
ecosystem, it remains challenging to use both together. It might well
remain so, because the build, installation, and dependency tooling is so
disparate between the two communities.

To name the tool, we wanted to continue
the theme of Pacific islands begun by Fiji. With so many to choose from, we
needed a starting point, and we chose the geographic midpoint between Loïc's
base of San Francisco and mine of Melbourne. Of course that ends up being
in the middle of the ocean, but not too far from that is the tiny village of
Napari, in the Republic of Kiribati. So, that evening, in Loïc's apartment,
napari (lowercase n) was born.

The next week, at the Berkeley sprint, I finally met Kira Evans, a contributor to scikit-image
who had made several incredibly technical pull requests,
on my favourite topic of extending n-dimensional support, and whom I'd thus
asked Nelle to invite to the sprint.
On certain rare occasions, you make decisions that you look back on and think something like,
hot damn that was a good decision! Inviting Kira is one of those for me.

Thanks to [VisPy](http://vispy.org/), Loïc was able to hack out a quick
prototype that week. But starting a new lab from scratch is hard work, and he
knew he would not have time to develop it further. He did, however, have some
funds available for a summer intern, and he offered it to Kira, who as a result is now officially
a college dropout and CZI software engineer. That summer, under the guidance of Loïc, Stéfan
van der Walt, and myself, Kira put together the first implementation of napari as you see
it today.

By the fall, although internally napari was quite a mess, as tends to happen in
new, fast-growing projects, functionally it was impressive enough that Jeremy Freeman and
Nick Sofroniew, from CZI's Computational Biology division, started paying close attention. 
Initially, Nick brought some brilliant management to napari, instituting weekly meetings
that he drove (and continues to drive) like a champion. Pretty soon, though, he made his
first code contribution to napari, and after that it was like seeing a racecar take off.
No one has done more for napari than Nick, and Loïc and I owe him an enormous debt of
gratitude for that.

# napari now



# Our vision for the future


