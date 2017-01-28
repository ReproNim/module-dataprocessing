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
Continuous Integration is a software development practice where members of a team test and integrate their work frequently against a controlled source code repository. The main benefit of CI are reduced risk of long integration process at the end of a project and easier to find and remove bugs.

The best CI practices evolve over time as new technology and techniques are introduced, 
but these are selected principles of CI:

 * Maintain a code repository
The team should use a version control system, 
that allows for easy tracking all changes of the project's source code. 
Most open source projects use Git as a version control system and Github as a web hosting service
(TODO link to Yarik's part).


 * Automate the build and testing
Building of the system, that includes compaling, linking and other processes that are required 
to execute the program, should be triggered by a single command line. 
Build tools, such as `make`, can help to automate the build.

In practice, if you use Python or other interpreted languages, 
code does not need to be compiled. 
You still might need to remember about the software dependencies. (TODO: is it the place for mentioning requirements?)
For Python projects requirements files, that contain a list of items to be installed,
are often used. A typical structure of a file called `requirements.txt` is:

~~~
numpy>=1.6.2
scipy>=0.11
nibabel>=2.0.1
simplejson>=3.8.0 
pytest>=3.0
~~~

Once you have a requiremnet file, you can easily install all dependencies by running
`pip install -r requirements.txt`


In addition to the traditional build, that only assure the program runs,
unit testing should be incorporate into the build process to confirm that
the program behaves as we expect.
If you're using `pytest` for your Python project, you can add `py.test` command to
the building process.

* Commit changes and integrate with the main code frequently

As a developer you should try to commit at least once per dayin order to keep track 
of your changes.
By doing this frequently, you can find out if there's a conflict between your work 
and other developers work.
If the main branch is automatically build and tested, 
you can easily  detect conflicts in compilation and excecution of your code. 


For a full list of CI principles with detailed explanaition please check online resources:
- Wikipedia: https://en.wikipedia.org/wiki/Continuous_integration
- a nice review by Martin Fowler: https://www.martinfowler.com/articles/continuousIntegration.html
- a short blog post by Darryl Bowler: http://blogs.collab.net/devopsci/ten-best-practices-for-continuous-integration


### Travis CI 

Travis CI is a continuous integration service used to build and test software 
projects hosted at GitHub.
It’s commonly used for open source Python projects, that can use the service at no charge

In order to use Travis CI you have to sign in to the service with your GitHub account 
and link Travis CI with the GitHub projects you want to test. 
Please follow the instruction on the [Travis website] 
(https://docs.travis-ci.com/user/getting-started/) 
or from [a blog post](https://www.smartfile.com/blog/testing-python-with-travis-ci/).

In order to configure a Travis CI worflow a YAML format text file `.travis.yml`
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
in your requiremnet file:
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
a GitHub project. You might want to check  [Circle CI](https://circleci.com/)
or [Codeship](https://codeship.com/) platforms. 
If you're interested, you can find blog posts that compare these tools, e.g by
[Alex Gorbatchev](http://npmawesome.com/posts/2015-01-22-continuous-integration-in-the-cloud-comparing-travis-circle-and-codeship/).


### Other resources:
- An easy to follow software carpentry [lesson about testing](http://katyhuff.github.io/python-testing/index.html).