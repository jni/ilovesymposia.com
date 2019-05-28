<!--
.. title: Continuous integration in Python, 7: some helper tools and final thoughts
.. slug: continuous-integration-in-python-7-some-helper-tools-and-final-thoughts
.. date: 2014-10-27 07:03:52
.. tags: continuous integration,Planet SciPy,Python,test-driven development,programming
.. category: 
.. link: 
.. description: 
.. type: text
.. excerpt: It's time to draw my "continuous integration in Python" series to a close. This final post ties all six previous posts together and is the preferred write-up to share more widely and on which to provide feedback.

Almost everything I know about good Python development I've learned from [Stéfan van der Walt](https://github.com/stefanv), [Tony Yu](https://github.com/tonysyu), and the rest of the [scikit-image team](https://github.com/scikit-image/scikit-image/graphs/contributors). But a few weeks ago, I was trying to emulate the scikit-image CI process for my own project: [cellom2tif](https://github.com/jni/cellom2tif), a tool to liberate images from a [rather useless](http://www.openmicroscopy.org/site/support/bio-formats5/formats/cellomics.html) proprietary format. (I consider this parenthetical comment sufficient fanfare to announce the [0.2 release!](https://pypi.python.org/pypi/cellom2tif/0.2.0)) As I started copying and editing config files, I found that even from a complete template, getting started was not very straightforward. First, scikit-image has much more complicated requirements, so that a lot of the `.travis.yml` file was just noise for my purposes. And second, as detailed in the previous posts, a lot of the steps are not found or recorded anywhere in the repository, but rather must be navigated to on the webpages of GitHub, Travis, and Coveralls. I therefore decided to write this series as both a notetaking exercise and a guide for future CI novices. (Such as future me.)
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>It's time to draw my "continuous integration in Python" series to a close. This final post ties all six previous posts together and is the preferred write-up to share more widely and on which to provide feedback.

Almost everything I know about good Python development I've learned from <a href="https://github.com/stefanv">Stéfan van der Walt</a>, <a href="https://github.com/tonysyu">Tony Yu</a>, and the rest of the <a href="https://github.com/scikit-image/scikit-image/graphs/contributors">scikit-image team</a>. But a few weeks ago, I was trying to emulate the scikit-image CI process for my own project: <a href="https://github.com/jni/cellom2tif">cellom2tif</a>, a tool to liberate images from a <a href="http://www.openmicroscopy.org/site/support/bio-formats5/formats/cellomics.html">rather useless</a> proprietary format. (I consider this parenthetical comment sufficient fanfare to announce the <a href="https://pypi.python.org/pypi/cellom2tif/0.2.0">0.2 release!</a>) As I started copying and editing config files, I found that even from a complete template, getting started was not very straightforward. First, scikit-image has much more complicated requirements, so that a lot of the <code>.travis.yml</code> file was just noise for my purposes. And second, as detailed in the previous posts, a lot of the steps are not found or recorded anywhere in the repository, but rather must be navigated to on the webpages of GitHub, Travis, and Coveralls. I therefore decided to write this series as both a notetaking exercise and a guide for future CI novices. (Such as future me.)

<!-- TEASER_END -->

To recap, here are my six steps to doing continuous integration in Python with pytest, Travis, and Coveralls:

</p><ul>
<li><a href="http://ilovesymposia.com/2014/10/01/continuous-integration-0-automated-tests-with-pytest/">Write tests</a></li>
<li><a href="http://ilovesymposia.com/2014/10/02/continuous-integration-1-test-coverage/">Measure test coverage</a></li>
<li><a href="http://ilovesymposia.com/2014/10/13/continuous-integration-in-python-3-set-up-your-test-configuration-files/">Set up config files</a></li>
<li><a href="http://ilovesymposia.com/2014/10/15/continuous-integration-in-python-4-set-up-travis-ci/">Turn on Travis-CI</a></li>
<li><a href="http://ilovesymposia.com/2014/10/15/continuous-integration-in-python-5-report-test-coverage-using-coveralls/">Turn on Coveralls</a></li>
<li><a href="http://ilovesymposia.com/2014/10/17/continuous-integration-in-python-6-show-off-your-work/">Badge your repo</a></li>
</ul>

If you do all of the above at the beginning of your projects, you'll be in a really good place one, two, five years down the line, when many academic projects grow far beyond their original scope in unpredictable ways and end up with much broken code. (See this <a href="http://www.nature.com/news/2010/101013/full/467775a.html">wonderful editorial</a> by Zeeya Merali for much more on this topic.)

<h2>Reducing the boilerplate with PyScaffold</h2>

But it's a lot of stuff to do for every little project. I was about to make myself some minimal <code>setup.cfg</code> and <code>.travis.yml</code> template files so that I could have these ready for all new projects, when I remembered <a href="http://pyscaffold.readthedocs.org/en/latest/index.html">PyScaffold</a>, which sets up a Python project's basic structure automatically (<code>setup.py</code>, <code>package_name/__init__.py</code>, etc.). Sure enough, PyScaffold has a <a href="http://pyscaffold.readthedocs.org/en/latest/features.html#unittest-coverage"><code>--with-travis</code> option</a> that implements <em>all</em> my recommendations, including pytest, Travis, and Coveralls. If you set up your projects with PyScaffold, you'll just have to turn on Travis-CI on your GitHub repo admin and Coveralls on coveralls.io, and you'll be good to go.

<h2>When Travises attack</h2>

I've made a fuss about how wonderful Travis-CI is, but it breaks more often than I'd like. You'll make some changes locally, and ensure that the tests pass, but when you push them to GitHub, Travis fails. This can happen for various reasons:

<ul>
<li>your environment is different (e.g. NumPy versions differ between your local build and Travis's VMs).</li>
<li>you're testing a function that depends on random number generation and have failed to set the seed.</li>
<li>you depend on some web resource that was temporarily unavailable when you pushed.</li>
<li>Travis has updated its VMs in some incompatible way.</li>
<li>you have more memory/CPUs locally than Travis allows.</li>
<li>some other, not-yet-understood-by-me reason.</li>
</ul>

Of these, the first three are acceptable. You can use <a href="http://conda.pydata.org/docs/">conda</a> to match your environments both locally and on Travis, and you should <a href="http://blog.amvtek.com/posts/2014/Jun/20/making-good-use-of-random-in-your-python-unit-tests/">always set the seed</a> for randomised tests. For network errors, Travis provides a special function, <a href="http://blog.travis-ci.com/2013-05-20-network-timeouts-build-retries/"><code>travis_retry</code></a>, that you can prefix your commands with.

Travis VM updates should theoretically be benign and not cause any problems, but, in recent months, they have been a <a href="https://github.com/scikit-image/scikit-image/pull/1090#issuecomment-50983376">significant</a> <a href="https://github.com/scikit-image/scikit-image/pull/1100#issuecomment-53651935"> source</a> <a href="https://github.com/scikit-image/scikit-image/pull/1189#issuecomment-58500996">of</a> <a href="https://github.com/scikit-image/scikit-image/pull/1189#issuecomment-58610095">pain</a> for the scikit-image team: every monthly update by Travis broke our builds. That's disappointing, to say the least. For simple builds, you really shouldn't run into this. But for major projects, this is an unnecessary source of instability.

Further, Travis VMs don't have unlimited memory and disk space for your builds (naturally), but <a href="https://github.com/scikit-image/scikit-image/pull/1199#issuecomment-59448250">the limits are not strictly defined</a> (unnaturally). This means that builds requiring "some" memory or disk space randomly fail. Again, disappointing. Travis could, for example, guarantee some minimal specs that everyone could program against — and request additional space either as special exemptions or at a cost.

Finally, there's the weird failures. I don't have any examples on hand but I'll just note that sometimes Travis builds fail, where your local copy works fine every single time. Sometimes <a href="http://stackoverflow.com/a/21727193">rebuilding</a> fixes things, and other times you have to change some subtle but apparently inconsequential thing before the build is fixed. These would be mitigated if Travis allowed you to clone their VM images so you could run them on a local VM or on your own EC2 allocation.

<a href="/2014/10/screen-shot-2014-10-27-at-7-57-36-pm.png"><img src="https://ilovesymposia.files.wordpress.com/2014/10/screen-shot-2014-10-27-at-7-57-36-pm.png?w=450" alt="Heisenbug" width="450" height="84" class="size-large wp-image-597"></a> A too-common Travis occurrence: randomly failing tests

In all though, Travis is a fantastic resource, and you shouldn't let my caveats stop you from using it. They are just something to keep in mind before you pull <em>all</em> your hair out.

<h2>The missing test: performance benchmarks</h2>

Testing helps you maintain the correctness of your code. However, as Michael Droettboom <a href="https://www.youtube.com/watch?v=OsxJ5O6h8s0">eloquently argued</a> at SciPy 2014, all projects are prone to feature creep, which can progressively slow code down. <a href="https://spacetelescope.github.io/asv/">Airspeed Velocity</a> is to benchmarks what pytest is to unit tests, and allows you to monitor your project's speed over time. Unfortunately, benchmarks are a different beast to tests, because you need to keep the testing computer's specs and load constant for each benchmark run. Therefore, a VM-based CI service such as Travis is out of the question.

If your project has <em>any</em> performance component, it may well be worth investing in a dedicated machine only to run benchmarks. The machine could monitor your GitHub repo for changes and PRs, check them out when they come in, run the benchmarks, and report back. I have yet to do this for any of my projects, but will certainly consider this strongly in the future.

<h2>Some reservations about GitHub</h2>

The above tools all work great as part of GitHub's <a href="https://help.github.com/articles/using-pull-requests/">pull request (PR)</a> development model. It's a model that is easy to grok, works well with new programmers, and has driven massive growth in the open-source community. Lately, I recommend it with a bit more trepidation than I used to, because it does have a few high-profile detractors, notably Linux and git creator <a href="https://github.com/torvalds/linux/pull/17#issuecomment-5654674">Linus Torvalds</a>, and OpenStack developer <a href="https://julien.danjou.info/blog/2013/rant-about-github-pull-request-workflow-implementation">Julien Danjou</a>. To paraphrase Julien, there are two core problems with GitHub's chosen workflow, both of which are longstanding and neither of which shows any sign of improving.

First, <strong>comments on code diffs are buried by subsequent changes</strong>, whether the changes are a rebase or they simply change the diff. This makes it very difficult for an outside reviewer to assess what discussion, if any, resulted in the final/latest design of a PR. This could be a fairly trivial fix (colour-code outdated diffs, rather than hiding them), so I would love to see some comments from GitHub as to what is taking so long.

<a href="/2014/10/screen-shot-2014-10-27-at-5-06-29-pm.png"><img src="https://ilovesymposia.files.wordpress.com/2014/10/screen-shot-2014-10-27-at-5-06-29-pm.png?w=660" alt="GitHub's hidden PR comments" width="660" height="127" class="size-large wp-image-596"></a> Expect to see a lot of these when using pull requests.

Second, <strong>bisectability is broken by fixup commits.</strong> The GitHub development model is not only geared towards small, incremental commits being piled on to a history, but it actively encourages these with their per-commit badging of a user's <a href="https://help.github.com/articles/viewing-contributions-on-your-profile-page/#contributions-calendar">contribution calendar</a>. Fixup commits make bug hunting with <a href="http://git-scm.com/docs/git-bisect">git bisect</a> more difficult, because some commits will not be able to run a test suite at all. This could be alleviated by considering only commits merging GitHub PRs, whose commit message start with <code>Merge pull request #</code>, but I don't know how to get git to do this automatically (ideas welcome in the comments).

I disagree with Julien that there is "no value in the social hype [GitHub] brings." In fact, GitHub has dramatically improved my coding skills, and no doubt countless others'. For many, it is their first experience with code review. Give credit where it is due: GitHub is driving the current, enormous wave of open-source development. But there is no doubt it needs improvement, and it's sad to see GitHub's developers apparently ignoring their critics. I hope the latter will be loud enough soon that GitHub will have no choice but to take notice.

<h2>Final comments</h2>

This series, including this post, sums up my current thinking on CI in Python. It's surely incomplete: I recently came across a curious "Health: 88%" badge on Mitchell Stanton-Cook's <a href="https://github.com/mscook/BanzaiDB">BanzaiDB</a> README. Clicking it took me to the project's <a href="https://landscape.io/github/mscook/BanzaiDB/52">landscape.io page</a>, which appears to do for coding style what Travis does for builds/tests and Coveralls does for coverage. How it measures "style" is not yet clear to me, but it might be another good CI tool to keep track of. Nevertheless, since it's taken me a few years to get to this stage in my software development practice, I hope this series will help other scientists get there faster.

If any more experienced readers think any of my advice is rubbish, please speak up in the comments! I'll update the post(s) accordingly. CI is a big rabbit hole and I'm still finding my way around.</body></html>
