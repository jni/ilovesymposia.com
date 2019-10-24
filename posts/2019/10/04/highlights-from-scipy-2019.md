<!--
.. title: Highlights from SciPy 2019
.. slug: highlights-from-scipy-2019
.. date: 2019-10-04 08:01:15 UTC+10:00
.. tags: Planet SciPy, Python, scientific programming
.. category: 
.. link: 
.. description: 
.. type: text
.. status: draft
-->

## SciPy for big images

Scientific Python is an exciting space to be in when it comes to analysing
bigger-than-RAM and/or n-dimensional images, which is my main focus these
days. I presented my skan (skeleton analysis) library. Skeleton analysis is
about measuring the length of single-pixel-wide “skeletons” of objects in
images. But, since I actually used the topic as a trojan horse to instead
talk about Numba, I'll let you read more about skeleton analysis in my
[previous post](https://ilovesymposia.com/2016/12/20/numba-in-the-real-world/)
on the topic (which I wrote when I was only just starting skan), as well as
the [paper](https://peerj.com/articles/4312/) we wrote on the topic.

Talking about Numba, I took a perspective I borrowed from Andreas Klöckner's
[keynote](https://youtu.be/Zz_6P5qAJck) at SciPy 2016, which is: how fast is
fast enough? In particular, if you get a 100x speedup from using Numba, is
that a lot? Are you done? Or are you only scratching the surface?

You can get some *blistering* performance from Numba. When I first played with it, I tried to write a 
Unfortunately, modern processors are so complex that it is hard to get a
definitive answer of when you're done, but in my talk I explore some
back of the envelope calculations you can use to tell when you're *probably*
*not* done, as well as some strategies to improve performance further.

You can see my talk [here](https://youtu.be/0pUPNMglnaE), and find the
notebooks I used and other links at
[this repo](https://github.com/jni/skan-talk-scipy-2019)

There were three other talks I want to highlight on these same themes.

First, Philipp Hanslovsky from Janelia Research Campus presented Imglyb, a portemanteau of Imglib(2) and NumPy. [Imglib2]() is a Java framework for n-dimensional image analysis and and visualisation. NumPy is... Well, I don't need to introduce NumPy. Imglyb allows shared memory between the two, so that you can use the extensive Java image analysis and visualisation stack directly and efficiently on NumPy arrays. Philipp demonstrated that you can write to a NumPy array while live viewing it in a Java application, which was damn cool. Watch his talk [here](), and download and run his notebooks [here]().

Next, Matt McCormick from KitWare dove into the nitty gritty of implementing affine transformations for blocked (bigger than memory) images. 

## Tutorials

I had my own scikit-image tutorial to worry about (led by Stéfan van der Walt
and Josh Warner), but the first morning I helped at Land on Vector Spaces by
Lorena Barba and XXX, which was brilliant. You should look at it on
[YouTube](), find the materials on [GitHub](), and even build on them, as they
are licenced as [CC-BY](). The visualisations of transformations of 2D vector
spaces were simply gold, and I hope to reuse them in the next edition of
[Elegant SciPy](). 

Highlighted talks:
- Philipp
- Alistair

Nick Sofroniew gave a lightning talk on napari, an n-dimensional array viewer based on [vispy](). This project was born out of a conversation between Loïc Royer and I in 

- video to skan talk

## Sprints

As I [have said]() [again]() and [again](), the code sprints are truly the best part of SciPy. 


## References and links

- Using skan to analyse the osteocyte network in bone tissue:
- skan paper
