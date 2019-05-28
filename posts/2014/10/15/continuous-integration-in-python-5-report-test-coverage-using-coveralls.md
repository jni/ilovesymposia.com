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
In this series of posts, we've covered:
<ul>
 	<li><a href="http://ilovesymposia.com/2014/10/01/continuous-integration-0-automated-tests-with-pytest/">Automated testing with pytest</a></li>
 	<li><a href="http://ilovesymposia.com/2014/10/02/continuous-integration-1-test-coverage/">Measuring test coverage</a></li>
 	<li><a href="http://ilovesymposia.com/2014/10/13/continuous-integration-in-python-3-set-up-your-test-configuration-files/">Setting up test configuration files</a></li>
 	<li>Using Travis-CI to <a href="http://ilovesymposia.com/2014/10/15/continuous-integration-in-python-4-set-up-travis-ci/">run your tests automatically with each push</a></li>
</ul>
Today I will show you how to continuously check your test coverage using <a href="https://coveralls.io">Coveralls</a>.

Travis runs whatever commands you tell it to run in your <code>.travis.yml</code> file. Normally, that's just installing your program and its requirements, and running your tests. If you wanted instead to launch some nuclear missiles, you could do that. (Assuming you were happy to put the launch keys in a public git repository... =P)

The <a href="https://coveralls.io">Coveralls</a> service, once again free for open-source repositories, takes advantage of this: you just need to install an extra piece of software from PyPI, and run it after your tests have passed. Do so by adding the line <code>pip install coveralls</code> to your <code>before_install</code> section, and just the <code>coveralls</code> command to a new <code>after_success</code> section:

```yaml
language: python
python:
- "2.7"
- "3.4"
before_install:
- pip install pytest pytest-cov
- pip install coveralls
script:
- py.test
after_success:
- coveralls
```

This will push your coverage report to Coveralls every time Travis is run, i.e., <em>every time you push to your GitHub repository.</em> You will need to turn on the service by:
<ul>
 	<li>going to <a href="https://coveralls.io">coveralls.io</a> and signing in with your GitHub credentials</li>
 	<li>clicking on "Repositories", and "Add Repo" (and perhaps on "Sync GitHub Repos", top right)</li>
 	<li>switching your repository's switch to "On"</li>
</ul>
<img src="https://ilovesymposia.files.wordpress.com/2014/10/skitch-8.png" alt="Adding a repo to Coveralls">

That's it! With just two small changes to your <code>.travis.yml</code> and the flick of a switch, you will get continuous monitoring of your test coverage with each push, and also with each pull request. By default, Coveralls will even comment on your pull requests, so that you instantly know whether someone is not testing their submitted code!

Plus, you get a swanky web dashboard to monitor your coverage!

<img src="https://ilovesymposia.files.wordpress.com/2014/10/screen-shot-2014-10-16-at-3-49-30-pm.png" alt="Coveralls dashboard">

Tune in tomorrow for the final chapter in continuous integration in Python: showing off! No, it's not a blog post about blogging about it. =P

(Addendum: the above <code>.travis.yml</code> will run Coveralls <em>twice</em>, once per Python version, so it will also comment twice for your PRs, notify you twice, etc. If you want to test more Python versions, the problem multiplies. Therefore, you can choose to call Coveralls <em>only for a specific Python version</em>, like so:

```yaml
after_success:
- if [[ $ENV == python=3.4* ]]; then
coveralls;
fi
```

This is the approach taken by the scikit-image project.)

<strong>Update:</strong> <a href="http://ilovesymposia.com/2014/10/17/continuous-integration-in-python-6-show-off-your-work/">Volume 6: Badge your repo to show off your work</a>
