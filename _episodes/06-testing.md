---
title: "Lesson 5: Use integration testing to revalidate analyses as data and software change"
teaching: 2-5h
exercises: 2-5h
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


This lesson extends the software testing topic and introduces Continuous Integration 
workflow. 
Examples of Python testing frameworks and CI platforms will be used.   

### Lesson outline

- Element 1: Introduction to regression tests
- Element 2: Python testing frameworks
- Element 3: Regression tests for Simple Workflow
- Element 4: Continuous Integration practice
- Element 5: Continuous Integration Service: Travis CI


### Lesson requirements

It is essential to have a basic understanding of:
- Git and GitHub: [Software Carpentry materials (full:3h, familiarize: 30min)](http://swcarpentry.github.io/git-novice/); 
- Python (only some parts use Python): [Software Carpentry materials (full: 5h, familiarize: 1h)](http://swcarpentry.github.io/python-novice-inflammation/)


Although not essential it is helpful to have an understanding of:
- Unit tests: [Software Carpentry materials (full: 1:30h, familiarize: 20min)](http://katyhuff.github.io/python-testing/04-units/)


### Element 1: Introduction to regression tests

[Regression tests](https://en.wikipedia.org/wiki/Regression_testing)  verify that 
software previously developed and tested still performs correctly 
even after it was changed or interfaced with other software.
As opposed to writing [unit tests](https://en.wikipedia.org/wiki/Unit_testing) you don't have to 
knows the correct output, the assumption is that the past results are correct.

In science, regression tests perform a special role. 
Scientists often don't know what is the correct answer before doing the research, 
therefore results can't be compared to the prior known results.
However, you still want to be sure that your published results are not sensitive to 
an operating system, a specific version of library, etc.
If your results differ when you're changing computer environment, you should understand the 
source of the changes. 
The first step would be to automatically check the results with various environments
whenever you make any changes.
You can achieve this by combining your regression tests with a CI platform.

> ## Regression tests and future development
> Regression tests (as well as unit and any other type of tests) can help you with future
> development process.
> Whenever you're changing your code, by adding new functionality or improving performance,
> you can check if you're still able to reproduce your previous results.
> The results might change if you're improving the part of the code that is involved 
> in the analysis, but it's important to understand what aspect of the code change leads to 
> a change in output. 
> Thus regression tests can help associate changes in the output with changes in code,
> so guard you against inexplicable changes in your results.

> Moreover, writing regression tests that are based on your published results allow other
> scientists to verify easily their scientific approach, software used, or similar data set.
{: .callout}

> #### External teaching materials
> 
>  - the software carpentry provides more materials on 
> [unit tests (10 min)](http://katyhuff.github.io/python-testing/04-units/), 
> and [integration and regression tests (10min)](http://katyhuff.github.io/python-testing/07-integration/)
{: .callout}

### Element 2: Introduction to writing tests:  testing Python code with Pytest

Before we start writing regression tests we will show examples of simple Unit tests. 
We will use Python with pytest library as an example.  
If you are using Python we strongly recommend using
 [pytest](http://doc.pytest.org/en/latest/) for testing. 
If you are using other languages, you should check for appropriate testing libraries. 

The pytest framework makes it easy to write simple tests and allows you to use
the standard python assert for verifying your result. 
At the same time pytest scales well to support complex testing for whole libraries.

  * Usage examples can be found [here](http://doc.pytest.org/en/latest/usage.html).
  * Rules for standard test discovery can be found [here](http://doc.pytest.org/en/latest/goodpractices.html).
  * [Examples of tests using Pytest](http://doc.pytest.org/en/latest/example/index.html)
  * [More examples from Brian Okken website](http://pythontesting.net/framework/pytest/pytest-introduction/)

Let say we want to write a function that calculates 
[factorial](https://en.wikipedia.org/wiki/Factorial). 
In Python we can start from something like that:

```python

def my_factorial(n):
    if n == 1 or n == 0:
        return 1
    else:
        return n * my_factorial(n-1)
```
It is rather simple recursive function and it might be hard to think about mistakes, but we should still
write some tests. 
Let's choose a couple of values of `n` that we know answers for:

```python
def test_factorial():
    assert my_factorial(1) == 1
    assert my_factorial(5) == 120
```

Our function should pass the test, but we can think if this is enough. 
It is always good idea to test various values of arguments and try to include limits.
We already have the lowest limit, `1`, but if we want to use our function for large numbers
(let say up to 10000), we should also check for those. 
We might not know the correct answer, but we can still check
if the function is working and the result is larger than 1:

```python
def test_factorial_large():
    assert my_factorial(10000) > 0
```

If you run this test, it is very likely that it fails with 
`RuntimeError: maximum recursion depth exceeded`, 
even though the algorithm is correct. 
This is simple due to the Python way of handling recursion.
So we should think how to rewrite the code, so our tests pass. 
One way would be to remove recursion, e.g.:

```python

def my_factorial(n):
    result = 1
    for i in range(1, n+1):
	result = result * i
    return result
```

Now, our function works for small and large values! 
But what about negative values? 
We don't want to calculate factorials for negative values, but we didn't include this
information within our code and if we ask for `my_factorial(-10)`, we will get `1`
and this is not what we expect.
In fact, we would like our function to raise an exception and tell us 
that we should not provide a negative values of `n`.
This requirement can be also implemented within a test, e.g.:


```python
import pytest

def test_factorial_negative():
    with pytest.raise(Exception):
       my_factorial(-10)
```

This test simply checks if any exception is raised when we call the function for `-10`.
If we try to run this test it should fail.
We can fix it by checking the values of `n` at the beginning of the function:

```python

def my_factorial(n):
    if n < 0:
        raise Exception("factorial can be calculated only for positive numbers")

    result = 1
    for i in range(1, n+1):
        result = result * i
    return result
```

Now all the test should be passing, and the current function is much better than the one 
we wrote at the very beginning.



> ## Hands on exercise:
>
>  Write a simple unit test for `creating dataframe` function from `check_output.py` or any other function 
>  from  Simple Workflow.
>
> > ## Exemplary solution
> > 
> > Create an exemplary directory `output_1` with subdirectories `sub_a`, `sub_b` and `sub_c`. 
> > Each of these subdirectories should contain a `data.json` file with a dictionary that has at least 
> > one element: `{"first": [1, 11]}`.
> > 
> > ```python
> > import sys, os, glob
> > import pandas as pd
> > from check_output import creating_dataframe
> > 
> > def test_creating_dataframe():
> >     filename_list = sorted(glob.glob('output_1/*/data.json'))
> > 
> >     data_frame = creating_dataframe(filename_list)
> > 
> >     assert (data_frame.first_volume.keys() == ['sub_a', 'sub_b', 'sub_c']).all()
> >     assert (data_frame.first_volume == [11., 33, 55]).all()
> >     assert (data_frame.first_voxels == [1., 3, 5]).all()
> >```
> {: .solution}
{: .challenge}


> ## Hands on exercise:
>
>  Change the previous test so it checks the results for more than one directory,
> use `@pytest.mark.parametrize` decorator.
>
> > ## Exemplary solution
> >
> > Create an exemplary directory `output_1` with subdirectories `sub_a`, `sub_b` and `sub_c`.
> > Each of these subdirectories should contain a `data.json` file with a dictionary that has at least
> > one element: `{"first": [1, 11]}`.
> >
> > ```python
> > import sys, os, glob
> > import pandas as pd
> > from check_output import creating_dataframe
> >
> > @pytest.mark.parametrize(directory_name, keys, volume_list, voxel_list, [
> >                           ("output_1", ['sub_a', 'sub_b', 'sub_c'], [11, 33, 55], [1, 3, 5]),
> >                           ("output_2", ['subject_1', 'subject_2'], [8.5, 3.], [11.4, 3.5]),
> >                          ])			      
> > def test_creating_dataframe(directory_name, keys, volume_list, voxel_list):
> >     filename_list = sorted(glob.glob(os.path.join(directory_name, '/*/data.json')))
> >
> >     data_frame = creating_dataframe(filename_list)
> >
> >     assert (data_frame.first_volume.keys() == keys).all()
> >     assert (data_frame.first_volume == volume_list).all()
> >     assert (data_frame.first_voxels == voxel_list).all()
> >```
> {: .solution}
{: .challenge}

> #### External teaching materials
>
> - You can find a very good introduction to all Python test frameworks in
> - [Brian Okken introduction](http://pythontesting.net/start-here/) (full: 5h, familiarize pytest: 1h).
{: .callout}


### Element 3: Regression tests for Simple Workflow

In the [Simple Workflow](https://github.com/ReproNim/simple_workflow) 
repository you can find directory with the 
[expected output](https://github.com/ReproNim/simple_workflow/tree/master/expected_output).
Having the expected output we can write a simple regression tests that compares result for one 
subject with the results provided in the repository.


> ## Hands on exercise:
>
>  Write a simple regression test to compare results with expected results for one subject. 
>
> > ## Exemplary solution
> >
> > The following solution is written using Python and use instructions from  
> > [README file](https://github.com/ReproNim/simple_workflow/blob/master/README.md).
> >
> > Note that for comparing results of numerical computation we often do not check if the
> > numbers match exactly, but we specify absolute and/or relative tolerance.
> > 
> > ```python
> > import json
> > import numpy as np
> > import subprocess as sp
> > 
> > def test_comparison():
> >     # calling python script from command line 
> >     sp.call(["python", "run_demo_workflow.py", "--key", "11an55u9t2TAf0EV2pHN0vOd8Ww2Gie-tHp9xGULh_dA", "-n", "1", "-o", "my_output"])
> > 
> >     # a new file created by the script
> >     new_filename = "my_output/AnnArbor_sub16960/segstats.json"
> >     # the referential file (that has been probably created using different environment) 
> >     expected_filename = "expected_output/AnnArbor_sub16960/segstats.json"
> > 
> >     with open(new_filename, 'r') as fp:
> >         new_output = json.load(fp)
> > 
> >     with open(expected_filename, 'r') as fp:
> >         expected_output = json.load(fp)
> > 
> >     # comparing results from both files using numpy allclose function
> >     for key, val in new_output.items():
> >         assert np.allclose(expected_output[key], val, rtol=5e-02)
> > ```
> {: .solution}
{: .challenge}

### Element 4: Overview of Continuous Integration
Continuous Integration is a practice commonly used by members of software development teams
 that contains testing and integrating their work frequently against a controlled source code repository. 
The main benefit of CI are reduced risk of long integration process at the end 
of a project, and easier to find and remove bugs.


CI requires to use a version control system, 
that allows for easy tracking all changes of the project's source code. 
Most open source projects use Git as a version control system and Github as a web hosting service.

In order to automate the testing, the building process should be easy and fully automated,
that can be achieved by using tools like `make`.
If you use Python or other interpreted languages, the code does not need to be compiled, but 
you still have to remember about the software dependencies. 
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

More about `pip` and `requiremnts.txt` you can find on [pip website](https://pip.pypa.io/en/stable/user_guide/)

Alternatively, you might want to use [conda](https://conda.io/docs/index.html) that is 
an open source package management system and environment management system. 
Then, you can create an environment file `environment.yml` that specifies name, channels
and dependencies. 
The simplest examples of the environment file can look like this:

~~~
name: stats
dependencies:
  - numpy
  - pandas
~~~

The environment file from Simple Workflow can be found [here](https://github.com/ReproNim/simple_workflow/blob/master/environment.yml).

In order to create a conda environment based on the requirements you should run:

~~~
conda env create -f environment.yml
~~~

More about the conda environments can be found [here](https://conda.io/docs/using/envs.html).


In addition to the traditional build, that only assure the program runs,
unit and regression tests should be incorporate into the build process to confirm that
the program behaves as we expect.
If you're using `pytest` for your Python project, you can add `py.test` command to
the building process.

Building and testing should be done using various environments you want to support, 
e.g. Python 2.7 and Python 3.5.


A common practice is that every single pull request to the main branch of
the project repository is automatically built and tested before merging. 
That way, the team can easily  detect conflicts in compilation and execution of the code. 

> #### External readings
>
> For a list of CI principles with detailed explanation you can check online resources:
> - [Wikipedia](https://en.wikipedia.org/wiki/Continuous_integration)
> - A nice review by [Martin Fowler](https://www.martinfowler.com/articles/continuousIntegration.html)
> - A short blog post by [Darryl Bowler](http://blogs.collab.net/devopsci/ten-best-practices-for-continuous-integration)
{: .callout}

### Element 4: Continuous Integration Service: Travis CI 

[Travis CI](https://travis-ci.org/) is a continuous integration service used to build 
and test software projects hosted at GitHub.
It’s commonly used for open source Python projects that can use the service at no charge.

In order to use Travis CI you have to sign in to the service with your GitHub account 
and link Travis CI with the GitHub projects you want to test. 
Follow the instruction on the [Travis website] 
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

Check the [Python projects specific guide](https://docs.travis-ci.com/user/languages/python/)
for more examples.
Travis CI can also run and build Docker images, check the 
[Travis website](https://docs.travis-ci.com/user/docker/) for more information.

> ## Hands on exercise:
>
> Create a github repository with a file
> that contains `my_factorial` function and the tests we wrote.
> Next, create a `.travis.yml` file that runs the test.
> Afterward, open a Travis account and add your repository, so the tests are run automatically.
>
> > ## Exemplary solution
> >
> > `.travis.yml` can look like:
> > ~~~
> > language: python
> > python:
> >   - "2.7"
> >   - "3.5"
> > 
> > script: py.test
> > ~~~
> >
> >
> {: .solution}
{: .challenge}



> ## Hands on exercise:
>
> Create a github repository with a  file
> that contains `creating_dataframe` function and the test you wrote in previous parts.
> Next, create a `requiremnts.txt` and `.travis.yml` files, that runs the test.
> Afterward, open a Travis account and add your repository, so the tests are run automatically.
>
> > ## Exemplary solution
> >
> > `requiremnts.txt` can look like this:
> >
> > ~~~
> > json
> > pandas
> > pytest
> > ~~~
> >
> {: .solution}
{: .challenge}


 * Travis CI is not the only continuous integration service that can be used with 
a GitHub repository. 
Simple Workflow project uses [Circle CI servis](https://circleci.com/). 
You might also want to check [Codeship](https://codeship.com/) platforms. 

> #### External readings
> If you're interested, you can find blog posts that compare these tools, e.g by
> [Alex Gorbatchev](https://strongloop.com/strongblog/node-js-travis-circle-codeship-compare/).
{: .callout}

> #### External teaching materials
> - An easy to follow software carpentry [lesson about testing](http://katyhuff.github.io/python-testing/index.html) (full: 1:30h, familiarize: 20min).
> - [Presentation from Nipype workshop](http://nipy.org/workshops/2017-03-boston/lectures/lesson-testing/#1) (full: 1h, familiarize: 20min).
{: .callout}

> ## Hands on exercise:
>
>  - Fork the Simple Workflow repository and clone your own fork to your computer.
>  - Add your tests to the repository
>  - Understand `circle.yml` file and change it to incorporate your tests into the test part.

