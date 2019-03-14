<!--
.. title: Automatically resume interrupted downloads in OSX with curl
.. slug: automatically-resume-interrupted-downloads-in-osx-with-curl
.. date: 2013-04-11 05:25:48
.. tags: programming
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>I recently found myself at the wrong end of a crappy internet connection and needing to download a 97MB file. Safari and Chrome would both choke during the download and leave me with a unusable, truncated 4MB file, with no way to resume the download. The OSX command-line download utility, <span style="font-family:courier new;">curl</span>, can resume downloads, but I had to check manually each time the download was interrupted and re-run the command.

After typing "up, enter" more times than I care to admit, I decided to figure out the automatic way. Here's my bash one-liner to get automatic download resume in <span style="font-family:courier new;">curl</span>:

<code>export ec=18; while [ $ec -eq 18 ]; do /usr/bin/curl -O -C - "http://www.example.com/a-big-archive.zip"; export ec=$?; done
</code>

Explanation: the exit code <span style="font-family:courier new;">curl</span> chucks when a download is interrupted is 18, and <span style="font-family:courier new;">$?</span> gives you the exit code of the last command in bash. So, while the exit code is 18, keep trying to download the file, maintaining the filename (-O) and resuming where the previous download left off (-C).

<strong>Update:</strong> As Jan points out in the comments, depending what is going wrong with the download, your error code may be different. Just change "18" to whatever error you're seeing to get it to work for you! (If you're feeling adventurous, you could change the condition to <code>while [ $ec -ne 0 ]</code>, but that feels like using a <a href="https://docs.python.org/2/howto/doanddont.html#except">bare except</a> in Python, which is bad. ;)</p></body></html>
