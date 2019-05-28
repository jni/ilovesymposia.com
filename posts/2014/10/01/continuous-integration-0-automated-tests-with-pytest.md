<!--
.. title: Continuous integration in Python, Volume 1: automated tests with pytest
.. slug: continuous-integration-0-automated-tests-with-pytest
.. date: 2014-10-01 01:40:41
.. tags: continuous integration,Planet SciPy,Python,test-driven development,programming
.. category: 
.. link: 
.. description: 
.. type: text
.. status: published
-->

(<strong>Edit</strong>: I initially thought it would be cute to number from 0. But it turns out it becomes rather obnoxious to relate English (first, second, …) to 0-indexing. So this was formerly volume 0. But everything else remains the same.)

I just finished the process of setting up continuous integration <em>from scratch</em> for one of my projects, <a href="https://github.com/jni/cellom2tif">cellom2tif</a>, a simple image file converter/liberator. I thought I would write a blog post about that process, but it has slowly mutated into a hefty document that I thought would work better as a series. I'll cover automated testing, test coverage, and how to get these to run automatically for your project with <a href="https://travis-ci.org">Travis-CI</a> and <a href="https://coveralls.io">Coveralls</a>.

Without further ado, here goes the first post: how to set up automated testing for your Python project using <a href="http://pytest.org/latest/">pytest</a>.
<h2>Automated tests, and why you need them</h2>
Software engineering is hard, and it's incredibly easy to mess things up, so you should write <em>tests</em> for all your functions, which ensure that nothing obviously stupid is going wrong. Tests can take a lot of different forms, but here's a really basic example. Suppose this is a function in your file, <code>maths.py</code>:

```python
def square(x):
    return x ** 2
```

Then, elsewhere in your package, you should have a file <code>test_maths.py</code> with the following function definition:

```python
from maths import square

def test_square():
    x = 4
    assert square(x) == 16
```

This way, if someone (such as your future self) comes along and messes with the code in <code>square()</code>, <code>test_square</code> will tell you whether they broke it.
<h2>Testing in Python with pytest</h2>
A whole slew of testing frameworks, such as <a href="https://nose.readthedocs.org/en/latest/">nose</a>, will then traverse your project, look for files and functions whose names begin with <code>test_</code>, run them, and report any errors or assertion failures.

I've chosen to use <a href="http://pytest.org/latest/">pytest</a> as my framework because:
<ul>
 	<li>it is a strict superset of both nose and Python's built-in unittest, so that if you run it on projects set up with those, it'll work out of the box;</li>
 	<li>its output is more readable than nose's; and</li>
 	<li>its <a href="http://pytest.org/latest/fixture.html#fixture">fixtures</a> provide a very nice syntax for test setup and cleanup.</li>
</ul>
For a quick intro to pytest, you can watch Andreas Pelme's talk at this year's EuroPython conference:

<iframe width="560" height="315" src="https://www.youtube-nocookie.com/embed/LdVJj65ikRY" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

But the basics are very simple: sprinkle files named <code>test_something.py</code> throughout your project, each containing one or more <code>test_function()</code> definition; then type <code>py.test</code> on the command line (at your project root directory), and voila! Pytest will traverse all your subdirectories, gather up all the test files and functions, and run your tests.

Here's the output for the minimal <code>maths</code> project described above:

```
~/projects/maths $ py.test
============================= test session starts ==============================
platform darwin -- Python 2.7.8 -- py-1.4.20 -- pytest-2.5.2
collected 1 items

test_maths.py .

=========================== 1 passed in 0.03 seconds ===========================
```

In addition to the test functions described above, there is a Python standard called <a href="https://docs.python.org/2/library/doctest.html">doctest</a> in which properly formatted usage examples in your documentation are automatically run as tests. Here's an example:

```python
def square(x):
    """Return the square of a number `x`.

    [...]

    Examples
    --------
    >>> square(5)
    25
    """
    return x ** 2
```

(See my <a href="http://ilovesymposia.com/2014/01/09/best-practices-addendum-find-and-follow-the-conventions-of-your-programming-community/">post on NumPy docstring conventions</a> for what should go in the ellipsis above.)

Depending the complexity of your code, doctests, test functions, or both will be the appropriate route. Pytest supports doctests with the <code>--doctest-modules</code> flag. (This runs both your standard tests <em>and</em> doctests.)

```
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

<strong>Update:</strong> <a href="http://ilovesymposia.com/2014/10/02/continuous-integration-1-test-coverage/">Volume 2: measuring test coverage</a>
