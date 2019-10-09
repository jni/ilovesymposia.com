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
a decade. In 2009, I joined Mitya Chklovskii's lab and the FlyEM team at the
Janelia [Farm] Research Campus to work on the segmentation of 3D electron
microscopy (EM) volumes. I started out in Matlab, but moved to Python pretty
quickly and it was a very smooth transition (highly recommended! ;). Looking at
my data was always annoying though. I was either looking at single 2D slices
using `matplotlib.pyplot.imshow`, or saving the volumes in VTK format and
loading them into ITK-SNAP — which worked ok but required me to constantly be
changing terminals, and anyway was pretty inefficient because my disk ended up
cluttered with “temporary” VTK volumes.

I tried a bunch of stuff after that time, including Mayavi and even building my
own orthogonal views viewer with Matplotlib (which was super slow), but nothing
really clicked. What's worse, I didn't see any movement on this — I even found
a very discouraging thread on MNE-Python in which Gaël Varoquaux concluded,
given the lack of support, that 3D visualisation was not mission critical to
anyone and not worth pursuing further. (!) Throughout that time, I kept living
with either manually picking out 2D slices, or going back to tools outside of
Python, with all the context switching that that entailed. The end result is
that I was looking at my data far less than I should have been, which slowed
down my work. I can't count the number of silly mistakes I made and dead ends I
chased, just because I wasn't looking at the data enough.

# The beginning

In the meantime though, I was busy improving support for n-dimensional images
in scikit-image. In May last year, Nelle Varoquaux organised a joint sprint with
developers from scikit-learn, scikit-image, and dask at UC Berkeley, and I was
one of the lucky invitees. I'd recently seen that Loïc Royer, a good friend
from my Janelia days, had moved to San Francisco to start his lab at the Chan
Zuckerberg Biohub, so I asked him if I could crash at his place for a few days
before the sprint.

I knew Loïc as a Java guy. In Janelia he'd repeatedly extolled the virtues of
Java to me, and since then he'd worked on some incredible Java software,
including ClearVolume, a super-fast 3D viewer, and ClearControl, a super-fast
microscope control library (you might detect a theme there ;).  In fact,
although I love Python and its ecosystem and what we've built with
scikit-image, I've always been intimidated by Fiji and its ecosystem and its
~~10,000~~ 15,000 citations. Was I deluding myself that Python was important in
biology?

So I was both shocked and delighted when Loïc casually mentioned that he needed
a fast nD viewer in Python, and that we should build it together. Loïc had been
pushing the state of the art of deep learning in microscopy (and continues to
do so), and, despite the hard work of many to integrate Fiji with the major
deep learning learning libraries (which are Python), as well as the wider
scientific Python ecosystem, it remains challenging to use both together. It
might well remain so, because the build, installation, and dependency tooling
is so disparate between the two communities.  And so, that evening, in Loïc's
apartment, napari (lowercase n) was born (though it did not yet have a name).

Following on from his work on ClearVolume, Loïc and I agreed on some founding
principles for our tool:
- blazing fast (GPU-powered);
- native (neither of us likes opening a browser to work);
- n-dimensional first;
- native 2D and 3D viewing; and
- minimize new windows and pop-ups, choosing layering instead.

In terms of speed, we knew it was doable, thanks to Martin Weigert's excellent
3D volume viewer, [spimagine](https://github.com/maweigert/spimagine).

When choosing a name, after playing around with some terrible acronyms, we
instead decided to continue the theme of Pacific islands begun by Fiji (though
Fiji itself is a spectacular recursive acronym, “Fiji Is Just ImageJ”). With so
many to choose from, we needed a starting point, and we finally settled on the
geographic midpoint between Loïc's base of San Francisco and mine of Melbourne.
Of course that ends up being in the middle of the ocean, but not too far from
that point is the tiny village of Napari, in the Republic of Kiribati. We
thought it had a nice ring to it, and the name stuck.

The next week, at the Berkeley sprint, I finally met Kira Evans, a contributor
to scikit-image who had made several incredibly technical pull requests, on my
favourite topic of extending n-dimensional support, and whom I'd thus asked
Nelle to invite to the sprint. She had turned out to be a freshman at
Northeastern University, but despite her age she was already doing great things
in open source.  On certain rare occasions, you make decisions that you look
back on and think something like, hot damn that was a good decision! Inviting
Kira is one of those for me. (Nelle agreed.)

Thanks to [VisPy](http://vispy.org/), Loïc was able to hack out a quick
prototype that week. But starting a new lab from scratch is hard work, and he
knew he would not have time to develop it further. He did, however, have some
funds available for a summer intern, and after seeing the work she did that
week, he offered it to Kira. (As a result, she is now officially a college
dropout and full-time software engineer at CZI, the Chan Zuckerberg
Initiative.) That summer, under the guidance of Loïc, Stéfan van der Walt, and
myself, Kira put together the first implementation of napari as you see it
today.

By late summer/early fall, although internally napari was quite a mess, as
tends to happen in new, fast-growing projects, functionally it was already
impressive enough that Jeremy Freeman and Nick Sofroniew, from CZI's
Computational Biology division, started paying close attention.  Nick, who had
had similar experiences to mine working with Python and nD images, soon fell in
love with it, and literally could not wait for us to move it forward. He took
matters into his own hands.

Initially, Nick brought some brilliant management to napari, instituting weekly
meetings that he drove (and continues to drive) like a champion. Those meetings
were critical to bring others into the loop, including Ahmet Can Solak, Kevin
Yamauchi, Bryant Chunn, and Shannon Axelrod, who started to make pull requests
to improve napari.  Pretty soon, though, Nick made his own pull request, and
after that it was like seeing a racecar take off.  No one has done more for
napari than Nick, and Loïc and I owe him an enormous debt of gratitude for
that.

Soon after that, the [StarFISH team](https://github.com/spacetx/starfish) at
CZI adopted napari as their main image viewer, which again helped us to
discover bugs, improve our UI, and add features. By February, everyone in the
project was using it regularly, despite its warts. We started to think about
broadening the group of people with eyes on napari.

CZI was organising a meeting for grantees of its [Imaging
Scientists](https://go.chanzuckerberg.com/imaging) and [Imaging Software
Fellows](https://go.chanzuckerberg.com/imaging#imaging-software-fellows)
programs in March, which we thought would be a good opportunity to grow the
team. Among others, we invited John Kirkham (NVIDIA), Eric Perlman (then at
Johns Hopkins). The CZI meeting was a dramatic demonstration of the power of
bringing together people with complementary experience, as John added support
for viewing bigger-than-RAM [dask
arrays](https://docs.dask.org/en/stable/array.html) in about 20 minutes flat,
and Eric massively improved the performance of our segmentation visualisation
layer.

Meanwhile, this visit, as CZI had invited me, I was staying at a hotel downtown
rather than at Loïc's. One night, Loïc sent me an exasperated message that
still brings a smile to my face: “You know, if you'd stayed at your stupid
hotel a year ago, there would be no napari.” It does constantly amaze me how
many things had to line up for napari to exist, let alone be where it is today.

I came out of the March meeting thinking hey, this napari thing might have legs
on it! Not only had newcomers been able to improve it quickly without knowing
the codebase (which usually points to at least *decent* design), but we heard
over and over from the imaging scientists that viewing and annotating large
datasets remained a challenge for them.

So, we started gently expanding the user base, but in hushed tones, full of
caveats: “This software we're working on might be useful for you… If you're
brave and can work around all the missing functionality… Be sure to report any
issues…” One of our most frequent requests, which was indeed part of the napari
plan from the very beginning, was 3D volume rendering, and Pranathi Vemuri, a
data scientist at the Biohub, implemented that just in time for SciPy 2019. At
that conference, Nick presented napari to the public for the first time,
because many SciPy attendees tend to not mind working with development versions
of libraries. Indeed, we got contributions that week from Alex de Siqueira
(from the scikit-image team) and Matthias Bussonnier (IPython).

The biggest revelation for me, though, came immediately after SciPy, when I
went to Jackson Lab to teach a workshop on scikit-image. I've taught
scikit-image many times before, but for the first time, I went off-script and
analysed new data live, on IPython, rather than using Jupyter notebooks
pre-populated with toy data. I used napari instead of matplotlib, and found
napari's layers model *insanely* useful to visualise every step of the
analysis, one on top of the other, toggling their visibility on and off to
compare them. And it was super easy to add interactivity to the mix, clicking
on some points in napari, then grabbing those points to use as watershed seeds,
and popping the resulting segmentation as another layer to napari.

[screenshot: image, watershed seeds, watershed]

In the months since then, we've spent a lot of time improving the code, and
polishing the little edge cases, like making sure the coordinates reported by
the cursor are pixel perfect, merging the previously separate Image (2D) and
Volume (3D) layers, and extending 3D and multi-resolution support to all
layers.

Thanks to CZI's massive in-house investment in open source, we also actually
got an [audit](https://github.com/napari/napari/issues/469) for napari's GUI
from Lia Prins, a UX designer at CZI — an incredible opportunity that few
academic projects get. And, to repeat a familiar pattern, Nick immediately got
to work and implemented a bunch of fixes from the audit. To me, this update is
what's most pushed me to write this post, as the UI now feels more like a
coherent whole than the mishmash of features that results from organic growth
of a project.

# napari now

As I mentioned at the start, currently, everyone on the team uses napari on a
daily basis for their own work. Awesomely, our workflows actually look quite
different, yet napari meets the needs of all of them, in some form or another.
Last month we were surprised to find that Constantin Pape, at EMBL, was
developing his own viewer, Heimdall, on top of napari, completely independently
from us. I'm a big fan of Constantin's work, so little has brought me more joy
than finding his [repo](https://github.com/constantinpape/heimdall) and reading
this comment:

> most of the credit goes to napari; it's so great to finally have a decent
> python viewer.

So, what are those use cases?

## 1. Just *looking* at NumPy ndarrays quickly

This is my most common use case. I like to think about and use 2D, 3D, and 4D
images interchangeably. So, I often find myself calling `plt.imshow` on a 3D
array and being greeted by a `TypeError: Invalid dimensions for image data`,
*after* the figure window has popped up, which now stares back at me blank and
forlorn. No more, with `napari.view_image`. By default, napari will display the
last two dimensions of an array, and put in as many sliders as necessary for
the remaining dimensions.

To illustrate, I'll make a synthetic 4D array of blobs using scikit-image's
`binary_blobs` function:

```python
import numpy as np
import toolz as tz
from skimage import data, util


blobs_raw = np.stack([
    data.binary_blobs(length=64, n_dim=3, volume_fraction=f)
    for f in np.linspace(0.05, 0.5, 10)
])

add_noise = tz.curry(util.random_noise)
blobs = tz.pipe(
    blobs_raw,
    add_noise(mode='s&p'),
    add_noise(mode='gaussian'),
    add_noise(mode='poisson')
)

print(blobs.shape)
```

Now, I can look at the volume in napari. Thanks to VisPy and OpenGL, the
performance of the canvas is just blazing fast:

```python
import napari

viewer = napari.view_image(blobs, name='blobs')
```

[image]

If I want to see a 3D volume, for moderately sized arrays, one click suffices:
napari will change the number of displayed dimensions to 3, and remove one
of the sliders:

[image]

(This can also be achieved programmatically by modifying the dimensionality
model of the viewer as follows:

```python
viewer.dims.ndisplay = 3
```
)

Napari lets you explore your data quickly, which can dramatically accelerate
your work. Looking at these blobs slice by slice in matplotlib, for example, I
might not immediately see that they increase in number along axis 0, because I
would typically only look at one or two slices.

## 2. Overlaying computation results

Now, we can try some denoising on the image. Since I know that much of the
noise in this image is salt and pepper noise (randomly flipped bits), I can
try an [opening]() followed by a [closing]() as a first step. Because the
images along the 0th axis are independent, I don't want to denoise across them.
We can define a 3D (rather than 4D) neighbourhood as follows:

```python
from scipy import ndimage as ndi

neighbors3d = ndi.generate_binary_structure(3, connectivity=1)
neighbors = neighbors3d[np.newaxis, ...]

opening, closing = map(tz.curry, [ndi.grey_opening, ndi.grey_closing])

denoised = tz.pipe(
    blobs,
    opening(footprint=neighbors),
    closing(footprint=neighbors)
)
```

By default, napari will opaquely overlay one image over the next, with optional
adjustment of the transparency. In this case, that makes it difficult to
compare the results, because both layers have very high black and white
contrast.

```python
denoised_layer = viewer.add_image(denoised, name='denoised')
```

[image]

Instead, we can change the *blending mode*, how different layers appear on top
of each other, to "additive", and change the color of each layer so that
overlaps become easy to spot. These values are also accessible from the
`add_image` and `view_image` functions as keyword arguments, as well as from
the napari UI.

```python
blobs_layer = viewer.layers['blobs']
blobs_layer.blending = 'additive'
blobs_layer.colormap = 'magenta'
denoised_layer.blending = 'additive'
denoised_layer.colormap = 'cyan'
```

Now, any bright pixels in the original image that are gone in the new image
will be magenta, while bright pixels in the denoised image missing in the
original will be cyan. White pixels are bright in both images.

[image]

The denoised image looks much better! We can see that a lot of the grain is
removed in the denoised image. Now we can use some scikit-image and
SciPy functions to threshold and label the blobs:

```python
from skimage import filters

# label needs a shape `(3,) * ndim` array
neighbors2 = np.concatenate(
    (np.zeros_like(neighbors), neighbors, np.zeros_like(neighbors))
)

binary = filters.threshold_li(denoised) < denoised
labels = ndi.label(binary, structure=neighbors2)[0]

labels_layer = viewer.add_labels(labels, name='labeled', opacity=0.7)
```

[image]

We can quickly visualise the data and observe a phase transition from mostly
disconnected blobs to almost fully connected blobs as the volume fraction
crosses ~0.3.

## 3. Annotating data

Sometimes, it can be difficult to get an algorithm to exactly pick out what
you want in an image. With the right UI, however, annotation can be extremely
fast, and just a little interaction can dramatically help automated algorithms.

## 4. Viewing very large (dask) arrays

Thanks to John Kirkham's work, napari only loads the data that
it needs to display. Therefore, virtual arrays such as Dask arrays, HDF5
datasets, and zarr files can be loaded quickly and easily into napari, provided
that the individual slices are small enough.

Today's microscopes produce datasets ranging in the hundreds of GB to multiple
TB in size. Berkeley's Gokul Upadhyayula has made his lattice light sheet data
publicly available.
(Gokul's LLSM)

## 5. Quickly looking at images

```console
napari image.tif
```

```console
napari *.png
```

## 6. Parameter sweeps

https://gist.github.com/d-v-b/5dbdb7513d07e3360cbe5c0f3d5cacb6

## 7. More!

Nick has been working hard to improve our
[tutorials](https://github.com/napari/napari-tutorials), which will have more
details about all of the above. Meanwhile, Ahmet Can and Bryant are busy making
it easier to use napari to view images live from a microscope, or another
source. We look forward to hearing more use cases from you! Your first point of
contact to ask for help should be the [image.sc forum](https://forum.image.sc)
— don't forget to use the "napari" tag!  — as well as our [GitHub
issues](https://github.com/napari/napari) for bug reports and feature requests.

This might be a good time to emphasise that **napari is still in alpha**, which
means that we might change the API at any time. This is not to discourage you
from using it, but rather to point out that you should be ready to evolve
quickly with it, or pin to the current minor version.

# Our vision for the future

At the risk of sounding like a broken record, napari has already proven itself
insanely useful for all of us at the team, as well as a few extended members of
our community. But we are just getting started. We want to napari to help not
just Python practitioners, but also biologists and other scientists who want to
access Python's enormous scientific ecosystem.

We are [working](https://github.com/napari/napari/pull/263) on a plugin system
to allow the data from napari layers to be the input to Python functions.

[image: plugins menu]

We are doing this in
[collaboration](https://github.com/napari/napari/issues/140) with the
developers of [ImJoy](https://imjoy.io/) to make our plugins cross-compatible.

In addition to applying functions and seeing the outputs (what ImJoy's Wei
Ouyang refers to as *functional* plugins), we see napari as a base for more
complex interactivity, such as closed-loop learning in the style of
[Ilastik](https://www.ilastik.org/), skeleton tracing, 3D annotation, and more.
Indeed, we already provide a framework to add or modify keyboard shortcuts, and
are working on another to modify mouse interactivity.

Finally, we want to use Python's powerful introspection capabilities to make
every action recordable as valid Python. Someone once asked me whether we would
provide a headless mode for napari processing. Not really: napari-headless is
just Python. We think that napari could be a platform to teach
non-computational scientists the basics of Python, thus providing an
accelerated loop converting scientists from users to library and plugin
contributors.

# Join us!

In a little over a year, napari has grown beyond what I thought possible. But
we still have [a lot of work to do!](https://github.com/napari/napari/issues)
In addition to our [issues list](https://github.com/napari/napari/issues), we
have an [open roadmap](https://github.com/napari/napari/issues/420). And of
course, we encourage to you help the many open source projects we depend on,
including NumPy, SciPy, IPython, ImageIO, scikit-image, QtPy, VisPy, and
PyOpenGL. If you have OpenGL experience, VisPy in particular could use more
contributors!

# Acknowledgements
