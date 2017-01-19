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
{: .challenge}

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

_ a very good introduction to all Python test framework: http://pythontesting.net/start-here/

### Overview of Continuous Integration
- Motivation and introduction to Continuous Integration workflow (using: http://katyhuff.github.io/python-testing/08-ci.html, using: https://earldouglas.com/articles/python-ci.html)

### Travis CI with GitHub
- Intro to travis.yml file (using: https://docs.travis-ci.com/user/getting-started/ ; https://docs.travis-ci.com/user/customizing-the-build/)
- Travis.yml file for Python projects (using: https://docs.travis-ci.com/user/languages/python/,
https://www.smartfile.com/blog/testing-python-with-travis-ci/)

### CircleCI with GitHub
- Intro to circle.yml file (using: https://circleci.com/docs/gettingstarted/)
- circle.yml file for Python projects (using: https://circleci.com/docs/language-python/)
- Using Docker containers with Circle (using: https://circleci.com/docs/docker/)
