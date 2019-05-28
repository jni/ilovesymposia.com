<!--
.. title: Speed up your Mac's wake up time using pmset. Do it again after upgrading to Mavericks
.. slug: speed-up-your-macs-wake-up-time-using-pmset-do-it-again-after-upgrading-to-mavericks
.. date: 2013-11-05 20:42:53
.. tags: macbook pro,mavericks,osx,gadgets
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>Last year I got a 15" Retina Macbook Pro, an excellent machine. However, it was taking <em>way </em>longer than my 13" MBP to wake up from sleep. After a few months of just accepting it as a flaw of the new machines and the cost of being an early adopter, I finally decided to look into the problem. Sure enough, I came across this excellent post from OS X Daily:

<a href="http://osxdaily.com/2013/01/21/mac-slow-wake-from-sleep-fix/">Is Your Mac Slow to Wake from Sleep? Try this pmset Workaround</a>

Oooh, sweet goodness: basically, after 1h10min asleep, your Mac goes into a "deep sleep" mode that dumps the contents of RAM into your HDD/SSD and powers off the RAM. On wake, it needs to load up all the RAM contents again. This is slow when your machine has 16GB of RAM! Thankfully, you can make your Mac wait any amount of time before going into deep sleep. This will eat up your battery a bit more, but it's worth it. Just type this into the Terminal:
</p><pre>sudo pmset -a standbydelay 86400</pre>
This changes the time to deep sleep to 24h. Since I rarely spend more than 24h without using my computer, I now have instant-on every time I open up my laptop!

<!-- TEASER_END -->

Finally, the reason I wrote this now: upgrading to Mavericks sneakily resets your standbydelay to 4200. (Or, at least, it did for me.) Just run the above command again and you'll be set, at least until the next OS upgrade comes along!

<strong>Update:</strong> the original source of this tip appears to be <a href="http://www.ewal.net/2012/09/09/slow-wake-for-macbook-pro-retina/">a post</a> from Erv Walter on his site, Ewal.net. It goes into a lot more detail about the origin of this sleep mode — which indeed did not exist when I bought my previous Macbook Pro.</body></html>
