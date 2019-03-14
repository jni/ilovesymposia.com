<!--
.. title: My first use of Python 3's `yield from`!
.. slug: first-yield-from
.. date: 2015-10-19 22:19:35
.. tags: Legacy Python,Planet SciPy,programming,Python,Python 3
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>I never really understood why <code>yield from</code> was useful. Last weekend, I wanted to use Python 3.5's new <code>os.scandir</code> to explore a directory (and its subdirectories). Tragically, <code>os.scandir</code> is <em>not</em> recursive, and I find <code>os.walk</code>s 3-tuple values obnoxious.
Lo and behold, while I was trying to implement a recursive version of <code>scandir</code>, a <code>yield from</code> use just popped right out!

```
[code lang=python]
import os
def rscandir(path):
    for entry in os.scandir(path):
        yield entry
        if entry.is_dir():
            yield from rscandir(entry.path)
[/code]
```

That's it! I have to admit that reads wonderfully. The Legacy Python (aka Python 2.x) alternative is quite a bit uglier:

```
[code lang=python]
import os
def rscandir(path):
    for p in os.listdir(path):
        yield p
        if os.path.isdir(p):
            for q in rscandir(p):
                yield q
[/code]
```

Yuck. So, yet again: time to move away from Legacy Python! ;)</p></body></html>
