<!--
.. title: Continuous integration in Python, 5: report test coverage using Coveralls
.. slug: continuous-integration-in-python-5-report-test-coverage-using-coveralls
.. date: 2014-10-15 23:11:19
.. tags: continuous integration,Planet SciPy,Python,test-driven development,programming
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>In this series of posts, we've covered:

</p><ul>
<li><a href="http://ilovesymposia.com/2014/10/01/continuous-integration-0-automated-tests-with-pytest/">Automated testing with pytest</a></li>
<li><a href="http://ilovesymposia.com/2014/10/02/continuous-integration-1-test-coverage/">Measuring test coverage</a></li>
<li><a href="http://ilovesymposia.com/2014/10/13/continuous-integration-in-python-3-set-up-your-test-configuration-files/">Setting up test configuration files</a></li>
<li>Using Travis-CI to <a href="http://ilovesymposia.com/2014/10/15/continuous-integration-in-python-4-set-up-travis-ci/">run your tests automatically with each push</a></li>
</ul>

Today I will show you how to continuously check your test coverage using <a href="https://coveralls.io">Coveralls</a>.

Travis runs whatever commands you tell it to run in your <code>.travis.yml</code> file. Normally, that's just installing your program and its requirements, and running your tests. If you wanted instead to launch some nuclear missiles, you could do that. (Assuming you were happy to put the launch keys in a public git repository... =P)

The <a href="https://coveralls.io">Coveralls</a> service, once again free for open-source repositories, takes advantage of this: you just need to install an extra piece of software from PyPI, and run it after your tests have passed. Do so by adding the line <code>pip install coveralls</code> to your <code>before_install</code> section, and just the <code>coveralls</code> command to a new <code>after_success</code> section:

```https://coveralls.io
; then
          coveralls;
      fi

```

This is the approach taken by the scikit-image project.)

<strong>Update:</strong> <a href="http://ilovesymposia.com/2014/10/17/continuous-integration-in-python-6-show-off-your-work/">Volume 6: Badge your repo to show off your work</a></body></html>
