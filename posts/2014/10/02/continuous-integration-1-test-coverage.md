<!--
.. title: Continuous integration in Python, Volume 2: measuring test coverage
.. slug: continuous-integration-1-test-coverage
.. date: 2014-10-02 01:04:04
.. tags: continuous integration,Planet SciPy,Python,test-driven development,programming
.. category: 
.. link: 
.. description: 
.. type: text
.. has_math: no
.. status: published
.. wp-status: publish
-->

<html><body><p>(<strong>Edit</strong>: I initially thought it would be cute to number from 0. But it turns out it becomes rather obnoxious to relate English (first, second, ...) to 0-indexing. So this was formerly volume 1. But everything else remains the same.)

This is the second post in a series about setting up continuous integration for a Python project from scratch. For the first post, see <a href="http://ilovesymposia.com/2014/10/01/continuous-integration-0-automated-tests-with-pytest/">Automated tests with pytest</a>.

After you've written some test cases for a tiny project, it's easy to check what code you have automatically tested. For even moderately big projects, you will need tools that automatically check what parts of your code are actually tested. The proportion of lines of code that are run at least once during your tests is called your <em>test coverage</em>.

<!-- TEASER_END -->

For the same reasons that testing is important, measuring coverage is important. Pytest can measure coverage for you with the coverage plugin, found in the <a href="https://pypi.python.org/pypi/pytest-cov">pytest-cov</a> package. Once you've installed the extension, a test coverage measurement is just a command-line option away:

```
 ~/projects/maths $ py.test --doctest-modules --cov .
============================= test session starts ==============================
platform darwin -- Python 2.7.8 -- py-1.4.25 -- pytest-2.6.3
plugins: cov
collected 2 items 

maths.py .
test_maths.py .
--------------- coverage: platform darwin, python 2.7.8-final-0 ----------------
Name         Stmts   Miss  Cover
--------------------------------
maths            2      0   100%
test_maths       4      0   100%
--------------------------------
TOTAL            6      0   100%

=========================== 2 passed in 0.07 seconds ===========================
```

(The <code>--cov</code> takes a directory as input, which I find obnoxious, given that py.test so naturally defaults to the current directory. But it is what it is.)

Now, if I add a function without a test, I'll see my coverage drop:

```python
def sqrt(x):
    """Return the square root of `x`."""
    return x * 0.5
```

(The typo is intentional.)

```
--------------- coverage: platform darwin, python 2.7.8-final-0 ----------------
Name         Stmts   Miss  Cover
--------------------------------
maths            4      1    75%
test_maths       4      0   100%
--------------------------------
TOTAL            8      1    88%
```

With one more option, <code>--cov-report term-missing</code>, I can see which lines I <em>haven't</em> covered, so I can try to design tests specifically for that code:

```
--------------- coverage: platform darwin, python 2.7.8-final-0 ----------------
Name         Stmts   Miss  Cover   Missing
------------------------------------------
maths            4      1    75%   24
test_maths       4      0   100%   
------------------------------------------
TOTAL            8      1    88%   
```

Do note that 100% coverage does not ensure correctness. For example, suppose I test my <code>sqrt</code> function like so:

```python
def sqrt(x):
    """Return the square root of `x`.

    Examples
    --------
    >>> sqrt(4.0)
    2.0
    """
    return x * 0.5
```

Even though my test is correct, and I now have 100% test coverage, I haven't detected my mistake. Oops!

But, keeping that caveat in mind, full test coverage <em>is</em> a wonderful thing, and if you don't test something, you're guaranteed not to catch errors. Further, my example above is quite contrived, and in most situations full test coverage <em>will</em> spot most errors.

That's it for part 2. Tune in next time to learn how to turn on <a href="https://travis-ci.org">Travis</a> continuous integration for your GitHub projects!

<strong>Update:</strong> <a href="http://ilovesymposia.com/2014/10/13/continuous-integration-in-python-3-set-up-your-test-configuration-files/">Volume 3: set up config files.</a></p></body></html>
