<!--
.. title: Continuous integration in Python 3: set up your test configuration files
.. slug: continuous-integration-in-python-3-set-up-your-test-configuration-files
.. date: 2014-10-13 19:57:20
.. tags: continuous integration,Planet SciPy,Python,test-driven development,programming
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>This is the third post in my series on continuous integration (CI) in Python. For the first two posts, see <a href="http://ilovesymposia.com/2014/10/01/continuous-integration-0-automated-tests-with-pytest/">1: automated tests with pytest</a>, and <a href="http://ilovesymposia.com/2014/10/02/continuous-integration-1-test-coverage/">2: measuring test coverage</a>.

By now, you've got a bunch of tests and doctests set up in your project, which you run with the command:

```http://en.wikipedia.org/wiki/INI_file

omit = *script.py

```

Once you've done this, you can invoke your tests with a simple, undecorated call to just <code>py.test</code>.

To find out more about your configuration options, see the pytest <a href="http://pytest.org/latest/customize.html">basic test configuration</a> page, and Ned Batchelder's excellent <a href="http://nedbatchelder.com/code/coverage/config.html"><code>.coveragerc</code> syntax guide</a>.

That's it for this entry of my CI series. Follow this blog for the next two entries, setting up Travis CI and Coveralls.

<strong>Update:</strong> <a href="http://ilovesymposia.com/2014/10/15/continuous-integration-in-python-4-set-up-travis-ci/">Volume 4: Set up Travis-CI</a></p></body></html>
