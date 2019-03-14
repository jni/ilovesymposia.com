<!--
.. title: Use pmset to restore your Mac's instant-wake after upgrading to Yosemite
.. slug: use-pmset-to-restore-your-macs-instant-wake-after-upgrading-to-yosemite
.. date: 2014-09-22 04:14:14
.. tags: programming
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>I just upgraded to Mac OS X Yosemite Beta 3, and sure enough, Apple has <a href="http://ilovesymposia.com/2013/11/05/speed-up-your-macs-wake-up-time-using-pmset-do-it-again-after-upgrading-to-mavericks/">once again</a> broken my time-to-deep-sleep setting. After I upgraded, I noticed that my computer took a few seconds to wake from sleep, rather than the usual instant-on that defined OS X for so long. I checked <code>pmset</code>, Mac's power management command-line utility:

```http://ilovesymposia.com/2013/11/05/speed-up-your-macs-wake-up-time-using-pmset-do-it-again-after-upgrading-to-mavericks/

sudo pmset -a standbydelay 86400

```

Check in for a new post come next upgrade! (Mammoth?)</p></body></html>
