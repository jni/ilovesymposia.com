<!--
.. title: Continuous integration in Python, Volume 1: automated tests with pytest
.. slug: continuous-integration-0-automated-tests-with-pytest
.. date: 2014-10-01 01:40:41
.. tags: continuous integration,Planet SciPy,Python,test-driven development,programming
.. category: 
.. link: 
.. description: 
.. type: text
.. excerpt: I just finished the process of setting up continuous integration *from scratch* for one of my projects, [cellom2tif](https://github.com/jni/cellom2tif), a simple image file converter/liberator. I thought I would write a blog post about that process, but it has slowly mutated into a hefty document that I thought would work better as a series. I'll cover automated testing, test coverage, and how to get these to run automatically for your project with [Travis-CI](https://travis-ci.org) and [Coveralls](https://coveralls.io).

Without further ado, here goes the first post: how to set up automated testing for your Python project using [pytest](http://pytest.org/latest/).

.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>(<strong>Edit</strong>: I initially thought it would be cute to number from 0. But it turns out it becomes rather obnoxious to relate English (first, second, …) to 0-indexing. So this was formerly volume 0. But everything else remains the same.)

I just finished the process of setting up continuous integration <em>from scratch</em> for one of my projects, <a href="https://github.com/jni/cellom2tif">cellom2tif</a>, a simple image file converter/liberator. I thought I would write a blog post about that process, but it has slowly mutated into a hefty document that I thought would work better as a series. I'll cover automated testing, test coverage, and how to get these to run automatically for your project with <a href="https://travis-ci.org">Travis-CI</a> and <a href="https://coveralls.io">Coveralls</a>.

Without further ado, here goes the first post: how to set up automated testing for your Python project using <a href="http://pytest.org/latest/">pytest</a>.

</p><h2>Automated tests, and why you need them</h2>

Software engineering is hard, and it's incredibly easy to mess things up, so you should write <em>tests</em> for all your functions, which ensure that nothing obviously stupid is going wrong. Tests can take a lot of different forms, but here's a really basic example. Suppose this is a function in your file, <code>maths.py</code>:

```http://ilovesymposia.com/2014/01/09/best-practices-addendum-find-and-follow-the-conventions-of-your-programming-community/

~/projects/maths $ py.test --doctest-modules
============================= test session starts ==============================
platform darwin -- Python 2.7.8 -- py-1.4.20 -- pytest-2.5.2
collected 2 items

maths.py .
test_maths.py .

=========================== 2 passed in 0.06 seconds ===========================

```

<h2>Test-driven development</h2>

That was easy! And yet most people, my past self included, neglect tests, thinking they'll do them eventually, when the software is ready. This is backwards. You've probably heard the phrase "Test-driven development (TDD)"; this is what they're talking about: writing your tests <em>before</em> you've written the functionality to pass them. It might initially <em>seem</em> like wasted effort, like you're not making progress in what you actually want to do, which is write your software. But it's not:

https://twitter.com/zspencer/status/514447236239859712

By spending a bit of extra effort to prevent bugs down the road, you will get to where you want to go faster.

That's it for volume 1! Watch out for the next post: ensuring your tests are thorough by measuring <em>test coverage</em>.

<strong>Update:</strong> <a href="http://ilovesymposia.com/2014/10/02/continuous-integration-1-test-coverage/">Volume 2: measuring test coverage</a></body></html>
