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
>  - How to use Travis CI (or other CI platform) with your GitHub account?
>  - How to run Docker container with CI platform?


This lesson extends the software testing subject 
and introduces Continuous Integration workflow. Examples of Python testing frameworks 
and CI platforms will be used.   

### Lesson outline

- Element 1: Unit, integration and regression tests
- Element 2: Python testing frameworks
- Element 3: Continuous Integration practice
- Element 4: Continuous Integration Service: Travis CI


### Lesson requirements

It is essential to have a basic understanding of:
- Python
- Git and GitHub

Although not essential it is helpful to have an understanding of:
- Unit tests


### Unit, integration and regression tests

- [Unit tests](https://en.wikipedia.org/wiki/Unit_testing) isolate each part of the program and show that the individual parts are correct.

- [Integration tests](https://en.wikipedia.org/wiki/Integration_testing) combine individual software modules and test as a group. 

- [Regression tests](https://en.wikipedia.org/wiki/Regression_testing) verify that software previously developed and tested still 
  performs correctly even after it was changed or interfaced with other software. 
  For regression tests you don't have to knows what the expected result should be, 
  the assumption is that the past results were correct. 


- Resources: More about unit, integration and regression test you might find at the software carpentry [lesson](http://katyhuff.github.io/python-testing/index.html).



### Python testing frameworks
- Python has a few popular testing frameworks:
  * [unittest library](https://docs.python.org/2/library/unittest.html): 
a standard module that offers xUnit style framework.
  * [nose library](http://nose.readthedocs.io/en/latest/): 
a python unit test framework, no boilerplate, can run doctests, unittests.  
  * [pytest library](http://doc.pytest.org/en/latest/): 
a powerful python test framework, no boilerplate, easy to start working, 
    covers extensive options and testing features. 

- Resources: a very good introduction to all Python test frameworks by 
[Brian Okken](http://pythontesting.net/start-here/).


### Pytest 
If you don't have any specific reason to use other library we recomemnd using pytest. 
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


- The simplest execution:
~~~
$ pytest
or
$ py.test
~~~
  * More oubout usage and options can find [here](http://doc.pytest.org/en/latest/usage.html).
  * Rules for standard test discovery can find [here](http://doc.pytest.org/en/latest/goodpractices.html).


- Once you're familiar with basic examples, we recommend checking more advance examples. 
The good sources are:
  * [Brian Okken website](http://pythontesting.net/framework/pytest/pytest-introduction/)
  * [Pytest website](http://doc.pytest.org/en/latest/example/index.html)


### Overview of Continuous Integration
Continuous Integration is a software development practice where members of a team test 
and integrate their work frequently against a controlled source code repository. 
The main benefit of CI are reduced risk of long integration process at the end 
of a project and easier to find and remove bugs.

The best CI practices evolve over time as new technology and techniques are introduced, 
but these are selected principles of CI:

 * Maintain a code repository
The team should use a version control system, 
that allows for easy tracking all changes of the project's source code. 
Most open source projects use Git as a version control system and Github as a web hosting service
(TODO link to Yarik's part).


 * Automate the build and testing
Building of the system, that includes compiling, linking and other processes that are required 
to execute the program, should be triggered by a single command line. 
Build tools, such as `make`, can help to automate the build.

In practice, if you use Python or other interpreted languages, 
code does not need to be compiled. 
You still might need to remember about the software dependencies. (TODO: is it the place for mentioning requirements?)
For Python projects, requirements files that contain a list of items to be installed
are often used. A typical structure of a file called `requirements.txt` is:

~~~
numpy>=1.6.2
scipy>=0.11
nibabel>=2.0.1
simplejson>=3.8.0 
pytest>=3.0
~~~

Once you have a requirement file, you can easily install all dependencies by running

~~~
$ `pip install -r requirements.txt`
~~~

In addition to the traditional build, that only assure the program runs,
unit testing should be incorporate into the build process to confirm that
the program behaves as we expect.
If you're using `pytest` for your Python project, you can add `py.test` command to
the building process.

* Commit changes and integrate with the main code frequently

As a developer you should try to commit at least once per day in order to keep track 
of your changes.
You should also integrate your changes with the main code frequently.
By doing this, you can find out if there's a conflict between your work 
and other developers work.
If the main branch is automatically build and tested, 
you can easily  detect conflicts in compilation and execution of your code. 


For a full list of CI principles with detailed explanation you can check online resources:
- [Wikipedia](https://en.wikipedia.org/wiki/Continuous_integration)
- A nice review by [Martin Fowler](https://www.martinfowler.com/articles/continuousIntegration.html)
- A short blog post by [Darryl Bowler](http://blogs.collab.net/devopsci/ten-best-practices-for-continuous-integration)


### Continuous Integration Service: Travis CI 

[Travis CI](https://travis-ci.org/) is a continuous integration service used to build 
and test software projects hosted at GitHub.
It’s commonly used for open source Python projects that can use the service at no charge

In order to use Travis CI you have to sign in to the service with your GitHub account 
and link Travis CI with the GitHub projects you want to test. 
Please follow the instruction on the [Travis website] 
(https://docs.travis-ci.com/user/getting-started/) 
or from [a blog post](https://www.smartfile.com/blog/testing-python-with-travis-ci/).

In order to configure a Travis CI workflow a YAML format text file `.travis.yml`
has to be added to the root directory of your repository. 
The file specifies software environments you want to use to build and test your code.
The simplest `.travis.yml` for testing your project in Python 2.7 environment looks:
~~~
language: python
python:
  - "2.7"

script: py.test
~~~

If you want to add Python 3.5 environment and install dependencies included 
in your requirement file:
~~~
language: python
python:
  - "2.7"
  - "3.5"

install:
  - pip install -r requirements.txt

script: py.test
~~~
 
Please check the [Python projects specific guide](https://docs.travis-ci.com/user/languages/python/)
for more examples.
Travis CI can also run and build Docker images, please check the 
[Travis website](https://docs.travis-ci.com/user/docker/) for more information.

 * Travis CI is not the only continuous integration service that can be used with 
a GitHub repository. 
You might want to check  [Circle CI](https://circleci.com/)
or [Codeship](https://codeship.com/) platforms. 
If you're interested, you can find blog posts that compare these tools, e.g by
[Alex Gorbatchev](http://npmawesome.com/posts/2015-01-22-continuous-integration-in-the-cloud-comparing-travis-circle-and-codeship/).


### Other resources:
- An easy to follow software carpentry [lesson about testing](http://katyhuff.github.io/python-testing/index.html).

> ## Hands on exercise:
>
>  - Create a GitHub account if you don't have one.
>  - Create a new repository and clone it, so you have a local version.
>  - Login to Travis and link your repository. 
>  - Create a text file with a simple data, you can copy our example:
~~~
number of dogs, number of cats, female(1)/male(0), age
4, 0, 1, 33
0, 4, 1, 44
1, 1, 1, 21
0, 0, 0, 66
1, 0, 1, 47
2, 1, 0, 31
0, 2, 0, 22
2, 0, 1, 51
~~~
>  - Create `pets.py` file and function `mean(filename, pet_type)` that returns average age 
of cat/dog owners.  
>  - Create a `test_pets.py` and a test function that checks proper results of 
>   the `mean` function for `pet_type=dog` and `pet_type=cat`. 
>   You can try to use [`pytest.mark.parametrize` option](http://doc.pytest.org/en/latest/parametrize.html).
> - Run `pytest` locally and correct your `mean` function until all tests pass. 
> - Create `requiremnts.txt` to ask for pytest library (not older than 3.0 version) 
> and other libraries you might want to use. 
> - Create `.travis.yml` file, ask for at least two different python version, 
> install all libraries from `requiremnts.txt`, and run pytest to test your code. 
> - Add everything to the repository and push to your GitHub. 
> If Travis is set correctly, you should see that it builds various python environments 
> and tests your code in each of them.
> - If everything works you can come back to your local version of the repository, 
>  and think how would you like your code to behave if file doesn't contain data you expect
>  (e.g. raise exception if a file doesn't contain "number of dogs" or contain negative numbers,
>  remove a row that is incomplete)
> -  Create `test_input.py` and a few testing function that tests if your code behaves as you 
>  expect when file contain incorrect or incomplete data (you might want to create new text files 
>  with examples you want to test). Run pytest and improve your code until all test pass. 
> - Add new tests to repository and run them using Travis.
