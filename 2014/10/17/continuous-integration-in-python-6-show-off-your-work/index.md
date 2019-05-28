<!--
.. title: Continuous integration in Python, 6: Show off your work
.. slug: continuous-integration-in-python-6-show-off-your-work
.. date: 2014-10-17 06:37:37
.. tags: continuous integration,Planet SciPy,Python,test-driven development,programming
.. category: 
.. link: 
.. description: 
.. type: text
.. excerpt: Most high-profile open-source projects these days advertise their CI efforts. Above, I cheekily called this showing off, but it's truly important: anyone who lands on your GitHub page is a potential user or contributor, and if they see evidence that your codebase is stable and well-tested, they are more likely to stick around.
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>We're finally ready to wrap up this topic. By now you can:

</p><ul>
<li><a href="http://ilovesymposia.com/2014/10/01/continuous-integration-0-automated-tests-with-pytest/">Automatically test your code with pytest</a></li>
<li><a href="http://ilovesymposia.com/2014/10/02/continuous-integration-1-test-coverage/">Measure your test coverage</a></li>
<li><a href="http://ilovesymposia.com/2014/10/13/continuous-integration-in-python-3-set-up-your-test-configuration-files/">Configure your tests</a></li>
<li>Use Travis-CI to <a href="http://ilovesymposia.com/2014/10/15/continuous-integration-in-python-4-set-up-travis-ci/">run your tests automatically with each push</a></li>
<li>Use Coveralls to <a href="http://ilovesymposia.com/2014/10/15/continuous-integration-in-python-5-report-test-coverage-using-coveralls/">measure your coverage with each push</a></li>
</ul>

But, much as exercise is wasted if your bathroom scale doesn't automatically tweet about it, all this effort is for naught if visitors to your GitHub page can't see it!

Most high-profile open-source projects these days advertise their CI efforts. Above, I cheekily called this showing off, but it's truly important: anyone who lands on your GitHub page is a potential user or contributor, and if they see evidence that your codebase is stable and well-tested, they are more likely to stick around.

<!-- TEASER_END -->

Badging your README is easy. (You <em>do</em> have a README, <a href="">don't you?</a>) In Travis, go to your latest build. Near the top right, click on the "build passing" badge:

<img src="/2014/10/screen-shot-2014-10-17-at-8-20-00-pm.png" alt="Travis-CI badge">

You'll get an overlay, with a pull-down menu for all the different options for getting the badge. You can grab the image URL, or you can directly grab the Markdown to put into your markdown-formatted README, or a bunch of other options, including RST:

<img src="/2014/10/screen-shot-2014-10-17-at-8-27-05-pm.png" alt="Travis-CI badge URLs">

Just copy and paste the appropriate code and add it to your README file wherever you please.

Meanwhile, on your repository's Coveralls page, on the right-hand side, you will find another pull-down menu with the appropriate URLs:

<img src="/2014/10/screen-shot-2014-10-17-at-8-28-24-pm.png" alt="Coveralls badge URLs">

Again, just grab whichever URL is appropriate for your needs (I prefer Markdown-formatted READMEs), and add it to your README, usually next to the Travis badge.

And you're done! You've got <a href="http://ilovesymposia.com/2014/10/01/continuous-integration-0-automated-tests-with-pytest/">automated tests</a>, <a href="http://ilovesymposia.com/2014/10/02/continuous-integration-1-test-coverage/">tons of test coverage</a>, you're running everything correctly thanks to <a href="http://ilovesymposia.com/2014/10/13/continuous-integration-in-python-3-set-up-your-test-configuration-files/">configuration files</a>, and all this is getting run on demand thanks to <a href="http://ilovesymposia.com/2014/10/15/continuous-integration-in-python-4-set-up-travis-ci/">Travis</a> and <a href="http://ilovesymposia.com/2014/10/15/continuous-integration-in-python-5-report-test-coverage-using-coveralls/">Coveralls</a>. And thanks to badging, the whole world knows about it:

<img src="/2014/10/screen-shot-2014-10-18-at-12-33-37-am.png" alt="Readme badges">

<strong>Update:</strong> <a href="https://ilovesymposia.com/2014/10/27/continuous-integration-in-python-7-some-helper-tools-and-final-thoughts/">Volume 7: Some final thoughts, tips, and tools.</a></body></html>
