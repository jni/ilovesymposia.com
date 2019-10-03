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

I'm really excited to finally share a new(ish) project, napari, with the world. We have been developing napari [in the open](https://github.com/napari/napari) from the very first commit, but we didn't want to make any premature fanfare about it… Until now. It's still alpha software, but for months now, both the core napari team and a few collaborators/early adopters have been using napari in our daily work. I've found it *life-changing*.

# The background

I've been looking for a great nD volume viewer in Python for the better part of a decade. I started working on the segmentation of 3D electron microscopy (EM) volumes at the Janelia [Farm] Research Campus in 2009. I started out in Matlab, but moved to Python pretty quickly and it was a very smooth transition (highly recommended! ;). Looking at my data was always annoying though. I was either looking at single 2D slices using `matplotlib.pyplot.imshow`, or saving the volumes in VTK format and loading them into ITK-SNAP — which worked ok but required me to constantly be changing terminals, and anyway was pretty inefficient because my disk ended up cluttered with “temporary” VTK volumes.

I tried a bunch of stuff since then, including Mayavi and even building my own orthogonal views viewer with Matplotlib, but nothing really clicked. What's worse, I didn't see any movement on this — I even found a very discouraging thread on MNE-Python in which Gaël Varoquaux concluded, given the lack of support, that 3D visualisation was not mission critical to anyone and not worth pursuing further. (!) Throughout that time, I kept living with either manually picking out 2D slices, or going back to tools outside of Python, with all the context switching that that entailed. The end result is that I was looking at my data far less than I should have been, which slowed down my work. I can't count the number of silly mistakes I made and dead ends I chased, just because I wasn't looking at the data enough.

# The beginning

In the meantime though, I was busy improving support for n-dimensional images in scikit-image. In May last year, Nelle Varoquaux organised joint sprint with developers from scikit-learn, scikit-image, and dask at UC Berkeley, and I was one of the lucky invitees. I'd recently seen that Loïc Royer, a good friend from my Janelia days, had moved to San Francisco to start his lab at the Chan Zuckerberg Biohub, and I asked him if I could crash at his place for a few days before the sprint. I knew Loïc as a Java guy, and since we'd met at Janelia he'd worked on some incredible Java software, including ClearVolume, a 3D viewer.

We also happened to have a bit of extra budget to bring in scikit-image contributors, and, in what I consider one of my most inspired decisions, I suggested that we reach out to Kira Evans, a contributor to the project on GitHub who had made several incredibly technical pull requests to scikit-image, on my favourite topic of extending n-dimensional support. It was exactly the kind of contribution that would benefit from face-to-face interaction. She turned out to be a freshman at Northeastern 

# napari now

# Our vision for the future


