---
title: "Lesson 5: Use integration testing to revalidate analyses as data and software change"
teaching: ??
exercises: ??
questions:
- "Why and how do we use continuous integration?"
objectives:
- Understand the various types of tests
- Understand the Continuous Integration workflow 
keypoints:
- Continuous Integration makes software development more efficient.
- Continuous Integration platform can be easily used with a GitHub account.

---

> ## You can skip this lesson if you can answer these questions? --->
>
>  - What are the unit, integration and regression tests?
>  - What are the Python testing frameworks?
>  - Why do you use Continuous Integration (CI) in software development?
>  - What is the CI workflow?
>  - How to use Travis CI with your GitHub account?
>  - How to use CircleCI with your GitHub account?
>  - How to run Docker container on CircleCI?


This lesson extends the software testing subject 
and introduces Continuous Integration workflow. Examples of Python testing frameworks 
and CI platforms will be used.   

### Lesson outline

- Unit, integration and regression tests
- Python testing frameworks
- Overview of Continuous Integration
- Travis CI with GitHub
- CircleCI with GitHub

### Lesson requirements

It is essential to have a basic understanding of:
- Python
- Git and GitHub

Although not essential it is helpful to have an understanding of:
- Unit tests

### Unit, integration and regression tests

- Unit tests isolate each part of the program and show that the individual parts are correct.

- Integration tests combine individual software modules and test as a group. 

- Regression tests verify that software previously developed and tested still 
  performs correctly even after it was changed or interfaced with other software. 
  For regression tests you don't have to knows what the expected result should be, 
  the assumption is that the past results were correct. 

- Resources: http://katyhuff.github.io/python-testing/index.html, 
             https://en.wikipedia.org/wiki/Unit_testing,
             https://en.wikipedia.org/wiki/Integration_testing,
	     https://en.wikipedia.org/wiki/Regression_testing


### Python testing frameworks
- Python has a few popular testing frameworks:
  * unittest library: a standard module that offers xUnit style framework.
  * nose library: a python unit test framework, no boilerplate, can run doctests, unittests.  
  * pytest library: a powerful python test framework, no boilerplate, easy to start working, 
    covers extensive options and testing features. 

- Resources: a very good introduction to all Python test framework: http://pythontesting.net/start-here/


### Pytest 
If you don't have any specific reason to use other library we recomend using pytest. 
The pytest framework makes it easy to write simple tests and allows you to use 
the standard python assert for verifying your result.
At the same time pytest scales well to support complex testing for whole libraries.

- Examples of simple tests:
  
  * testing output of a function 
~~~
def half(a):
    return a/2

def test_div():
    assert half(3) == 1.5
~~~

  * checking list's elements
~~~
states = ["CA", "CO", "FL"]
def test_list():
    assert "MA" in states
~~~

  * checking type of an object
~~~
import numpy as np
a = np.array([[2, 3, 4], [12, 13, 5])]
def test_array_type():
    assert a.dtype == 'float64'
~~~


- The simplest excecution:
~~~
$ pytest
~~~
  * More oubout usage and options: http://doc.pytest.org/en/latest/usage.html
  * Rules for standard test discovery: http://doc.pytest.org/en/latest/goodpractices.html


- More examples and advance usage:
  * http://pythontesting.net/framework/pytest/pytest-introduction/
  * http://doc.pytest.org/en/latest/example/index.html


### Overview of Continuous Integration
"Continuous Integration is a software development practice where members of a team integrate their 
work frequently, usually each person integrates at least daily - leading to multiple integrations per day. 
Each integration is verified by an automated build (including test) to detect integration errors as 
quickly as possible." (https://www.martinfowler.com/articles/continuousIntegration.html)


- Motivation and introduction to Continuous Integration workflow (using: http://katyhuff.github.io/python-testing/08-ci.html, using: https://earldouglas.com/articles/python-ci.html)

### Travis CI with GitHub
- Intro to travis.yml file (using: https://docs.travis-ci.com/user/getting-started/ ; https://docs.travis-ci.com/user/customizing-the-build/)
- Travis.yml file for Python projects (using: https://docs.travis-ci.com/user/languages/python/,
https://www.smartfile.com/blog/testing-python-with-travis-ci/)

### CircleCI with GitHub
- Intro to circle.yml file (using: https://circleci.com/docs/gettingstarted/)
- circle.yml file for Python projects (using: https://circleci.com/docs/language-python/)
- Using Docker containers with Circle (using: https://circleci.com/docs/docker/)
