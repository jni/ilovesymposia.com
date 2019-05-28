<!--
.. title: Continuous integration in Python, 4: set up Travis-CI
.. slug: continuous-integration-in-python-4-set-up-travis-ci
.. date: 2014-10-15 04:00:59
.. tags: continuous integration,Planet SciPy,Python,test-driven development,programming
.. category: 
.. link: 
.. description: 
.. type: text
.. status: published
-->

<html><body><p>This is the fourth post in my Continuous integration (CI) in Python series, and the one that puts the "continuous" in CI! For the introductory three posts, see:

</p><ul>
<li><a href="http://ilovesymposia.com/2014/10/01/continuous-integration-0-automated-tests-with-pytest/">Automated testing with pytest</a></li>
<li><a href="http://ilovesymposia.com/2014/10/02/continuous-integration-1-test-coverage/">Measuring test coverage</a></li>
<li><a href="http://ilovesymposia.com/2014/10/13/continuous-integration-in-python-3-set-up-your-test-configuration-files/">Setting up test configuration files</a></li>
</ul>

<h2>Introduction to Travis-CI</h2>

Once you've set up your tests locally, it does you no good if you don't remember to run them! <a href="http://travis-ci.org/">Travis-CI</a> makes this seamless, because it will check out your code and run you tests for <em>each and every commit you push to GitHub!</em> (This is even more important when you are receiving pull requests on GitHub: the tests will be run online, without you having to individually check out each PR and run the tests on your machine!)

This is what continuous integration is all about. Once upon a time, the common practice was to pile on new features on a codebase. Then, come release time, there would be a feature freeze, and some time would be spent cleaning up code and removing bugs. In continuous integration, instead, no new feature is allowed into the codebase until it is bug free, as demonstrated by the test suite.

<h2>What to do</h2>

You need to first add a <code>.travis.yml</code> file to the root of your project. This tells Travis how to install your program's dependencies, install your program, and run the tests. Here's an example file to run tests and coverage on our <code>maths.py</code> sample project:

```yaml
language: python
python:
    - "2.7"
    - "3.4"
before_install:
    - pip install pytest pytest-cov
script:
    - py.test
```

Pretty simple: tell Travis the language of your project, the Python version (you can specify multiple versions, one per line, to test on multiple Python versions!), how to install test dependencies. Finally, the command to run your tests. (This can be anything, not just pytest or another testing framework, as long as a shell exit status of 0 means "success" and anything else means "failure".)

You can read more about the syntax and options for your <code>.travis.yml</code> in the <a href="http://docs.travis-ci.com/user/build-configuration/">Travis documentation</a>. There are other sections you can add, such as "virtualenv" to set up Python virtual environments, "install" to add compilation and other installation steps for your library, before testing, and "after_success" to enable e.g. custom notifications. (We will use this last one in the next post.)

Once you have created <code>.travis.yml</code> for your project, you need to turn it on for your repo. <strike>This is, currently, a fairly painful process, and I can't wait for the day that it's enabled by default on GitHub.</strike> [<strong>Update: see below</strong>] In the meantime though:

<ul>
<li><strike>Go to your project <a href="/2014/10/skitch-3.png">settings</a> page on GitHub.</strike></li>
<li><strike>Click on <a href="/2014/10/skitch-2.png">webhooks &amp; services</a>.</strike></li>
<li><strike>Click on <a href="/2014/10/skitch.png">add service</a>, searching for "Travis".</strike></li>
<li><strike>You'll be taken to the GitHub <a href="/2014/10/skitch-7.png">Travis configuration page</a>. Mine was pre-populated with the Travis token, but you might have to click on the link to your <a href="https://travis-ci.org/profile">Travis profile</a>, and click on the "Profile" tab to retrieve your token.</strike></li>
<li>In the <a href="https://travis-ci.org/profile">Travis profile page</a>, in the "Repositories" tab, click <a href="/2014/10/skitch-5.png">Sync now</a> to get your list of repos, and flick the switch for your current repo.</li>
</ul>

<i>[<strong>Update 2014-10-28:</strong> Thanks to <a href="https://twitter.com/hugovk">@hugovk</a> for pointing out that the first four points above can be skipped. It turns out that when you first log in to Travis-CI using your GitHub account, you give them write access to your webhooks. So, when you add a repo from their end, they go ahead and add themselves on the GitHub end! Boom. Way easier.]</i>

https://twitter.com/hugovk/status/526798595442618368

Voilà! Every push and pull-request to your repository will trigger a job on Travis's servers, building your dependencies and your software, running your tests, and emailing you if anything went wrong! Amazingly, this is completely free for open source projects, so you really have no excuse for not using it!

<img src="/2014/10/screen-shot-2014-10-15-at-9-55-05-pm.png" alt="Travis build page">

Follow this blog to learn how to continuously check test coverage using <a href="https://coveralls.io">Coveralls</a>, coming in the next post!

<strong>Update:</strong> <a href="http://ilovesymposia.com/2014/10/15/continuous-integration-in-python-5-report-test-coverage-using-coveralls/">Volume 5: turn on Coveralls</a></body></html>
