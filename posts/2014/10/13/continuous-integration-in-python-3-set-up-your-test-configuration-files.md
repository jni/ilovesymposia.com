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

This is the third post in my series on continuous integration (CI) in Python. For the first two posts, see <a href="http://ilovesymposia.com/2014/10/01/continuous-integration-0-automated-tests-with-pytest/">1: automated tests with pytest</a>, and <a href="http://ilovesymposia.com/2014/10/02/continuous-integration-1-test-coverage/">2: measuring test coverage</a>.

By now, you've got a bunch of tests and doctests set up in your project, which you run with the command:

```
$ py.test --doctest-modules --cov . --cov-report term-missing
```

<!-- TEASER_END -->

If you happen to have a few script and setup files lying around that you don't want to test, this might expand to

```
$ py.test --doctest-modules --cov . --cov-report term-missing --ignore script.py
```

As you can see, this is getting cumbersome. You can fix this by adding a <code>setup.cfg</code> file to the root of your project, containing configuration options for your testing. This follows standard <a href="http://en.wikipedia.org/wiki/INI_file">INI file syntax</a>.

```ini
[pytest]
addopts = --ignore script.py --doctest-modules --cov-report term-missing --cov .
```

Note that the "ignore" flag only tells pytest not to run that file during testing. You also have to tell the coverage module that you don't want those lines counted for coverage. Do this in a <code>.coveragerc</code> file, with similar syntax:

```ini
[run]
omit = *script.py
```

Once you've done this, you can invoke your tests with a simple, undecorated call to just <code>py.test</code>.

To find out more about your configuration options, see the pytest <a href="http://pytest.org/latest/customize.html">basic test configuration</a> page, and Ned Batchelder's excellent <a href="http://nedbatchelder.com/code/coverage/config.html"><code>.coveragerc</code> syntax guide</a>.

That's it for this entry of my CI series. Follow this blog for the next two entries, setting up Travis CI and Coveralls.

<strong>Update:</strong> <a href="http://ilovesymposia.com/2014/10/15/continuous-integration-in-python-4-set-up-travis-ci/">Volume 4: Set up Travis-CI</a>
