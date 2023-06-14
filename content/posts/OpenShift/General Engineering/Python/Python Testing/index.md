---
title: "Python Testing Frameworks: History, Comparison, and Usage"
date: 2023-06-11
author: "Nick Miethe"
description: "An overview of Python 3 testing frameworks, their history, features, and comparison, including example test cases and additional testing tools."
tags: ["Python", "Testing", "Framework", "Software Development", "unittest", "pytest", "doctest", "TDD"]
categories: ["Technical", "Introduction", "Guide"]
topics: ["Python", "Software Development"]
---

## Introduction

Hello, folks! It's your "meatiest" tech enthusiast, Nick Miethe, back again on [MeatyBytes.io](/). You've probably gathered that I spend most of my professional time these days working around [OpenShift](/posts/openshift); but you may not be aware that my 1st professional passion was **Python**! And there's something crucial about Python programming (or any programming, really) that can't be understated – testing. Hence, our subject today: **Python Testing Frameworks**.

### Testing Overview

You might be wondering, why all the fuss about testing? In the realm of software development, ensuring the quality and reliability of your code is paramount and *testing* is our first line of defense against the chaos of unpredictable results and code incompatibility. It watches over our code, ensuring that every component functions as expected, and alerts us when things start to go awry. Yet, all too often, testing is the 1st thing developers skip in a time crunch. I know I've been guilty of it *at least* a once or twice in the past!

![](py-test-frameworks.jpg "[Source](https://www.design-reuse.com/articles/53050/right-python-framework-selection-for-automation-testing.html) Example Py test frameworks")

### Post Synopsis

This is where testing frameworks come into play. Python, being a dynamic and versatile language, has several robust testing frameworks, such as `unittest`, `pytest`, and `doctest`, that aid developers in writing clean, efficient, and bug-free code. This post will delve into the history of testing in Python 3, highlight the most popular testing frameworks, present example test cases for each, and provide a comprehensive comparison of their features. My hope is that by understanding the testing landscape for Python, you will be more likely to include it in your next project!

Throughout the post, I'll be using my hands-on experience to guide you through the sometimes daunting world of Python testing. So, whether you're a newbie coder, an experienced *Pythonista*, or someone simply curious about the behind-the-scenes of programming, this post should have something for you.

## History of Testing in Python 3

Testing has been a part of Python since its early days. With the advent of Python 3, testing became even more integral to the language. In the initial versions of Python 3, the built-in `unittest` module, influenced by Java's JUnit, was the primary tool for developers to write and run tests. It provided a rich set of assertions and a test discovery mechanism which was a leap forward for testing in Python. Over time, other frameworks like `pytest` and `doctest` have emerged, offering additional capabilities and styles of testing.

## Popular Python 3 Testing Frameworks

In this section, we'll review our 3 Python Testing Frameworks: . As part of the overview for each framework, I've provided both a simple and detailed example. For the detailed example, we will be writing test cases for this Python 3 code snippet, inspired from my [GH Surveyor](https://github.com/miethe/gh-surveyor), of a function that populates a dict with information from url requests:

```python
def fetchRepoData(self, repo_path: str, repo_info: requests.Response):
    repo_json = repo_info.json()
    if repo_info.status_code == 200:
        self.data['repo_path'] = repo_path
        self.data['description'] = repo_json['description']
        self.data['html_url'] = repo_json['html_url']
        self.data['pushed_at'] = repo_json['pushed_at']
        self.data['stargazers_count'] = repo_json['stargazers_count']
        self.data['forks_count'] = repo_json['forks_count']
```

## pytest

### Overview

[`pytest`](https://docs.pytest.org/en/latest/) is a robust and feature-rich testing framework in Python that's used to write simple, as well as complex, functional test cases. It supports the creation of small, simple tests, but scales to support complex functional testing for applications and libraries.

### How it Works and Technologies Used

At its core, `pytest` uses Python's built-in assert statements, simplifying the syntax needed for writing tests. To run tests, you simply invoke `pytest` from the command line, and it will automatically discover and run tests in any files named `test_*.py` or `*_test.py` in your project.

`pytest` is built with a plugin architecture, and many of its features are actually implemented as plugins. This means that it's highly extensible, and you can add custom behavior by writing your own plugins. There are also many existing plugins available for things like parallel test execution, test coverage reporting, integration with other testing frameworks, and more.

### Differentiating Factors

One of the main differentiating factors of `pytest` is its powerful fixture system. Fixtures are a way to provide test data and test resources, and they can be modular and reusable across tests. They can also use dependency injection, where a test function's needed resources are specified as arguments, and pytest takes care of providing them.

`pytest` also has strong support for more complex test structures, such as parameterized tests and setup/teardown at various levels of scope (function, class, module, or entire test session). Its use of plain assert statements also makes test failures easy to read and debug, because it can show exactly what values caused the assert to fail.

### Simple Example

An example pytest test case might look like this:

```python
def test_sum():
    assert sum([1, 2, 3]) == 6, "Should be 6"
```

### Our Example

```python
import pytest
import requests
from mymodule import MyObject  # assuming the fetchRepoData method is in MyObject in mymodule.py

def test_fetchRepoData():
    repo_path = "some/repo/path"
    mock_response = requests.Response()
    mock_response.status_code = 200
    mock_response._content = b'{"description": "desc", "html_url": "url", "pushed_at": "push_time", "stargazers_count": 10, "forks_count": 5}'

    my_obj = MyObject()
    my_obj.fetchRepoData(repo_path, mock_response)

    assert my_obj.data['repo_path'] == repo_path
    assert my_obj.data['description'] == "desc"
    assert my_obj.data['html_url'] == "url"
    assert my_obj.data['pushed_at'] == "push_time"
    assert my_obj.data['stargazers_count'] == 10
    assert my_obj.data['forks_count'] == 5
```

## unittest

### Overview

[`unittest`](https://docs.python.org/3/library/unittest.html) is a testing framework that comes bundled with Python out of the box. It was inspired by the **xUnit** architecture that's found in many other languages, which means that it's based on object-oriented principles and requires tests to be bundled into test cases that are then managed by test suites.

### How it Works and Technologies Used

`unittest` uses a specific structure for tests. Tests are methods in subclasses of `unittest.TestCase`, and you use a variety of special assertion methods in the `TestCase` class, like `assertEqual(a, b)`, `assertTrue(x)`, `assertFalse(x)`, and others.

Tests can be grouped into test suites, and the framework includes tools for automatically discovering tests as well. You can run tests using a command line interface, or from within your own scripts.

### Differentiating Factors

The main differentiating factor of `unittest` is its strict structure and use of object-oriented principles. This can be a benefit or a drawback, depending on your needs. The structure can help keep tests organized and can be familiar to people coming from other languages with xUnit-style test frameworks.

### Simple Example

Here's an example of a simple `unittest` test case:

```python
import unittest

class TestSum(unittest.TestCase):
    def test_sum(self):
        self.assertEqual(sum([1, 2, 3]), 6, "Should be 6")

if __name__ == '__main__':
    unittest.main()
```

### Our Example

```python
import unittest
import requests
from unittest.mock import Mock
from mymodule import MyObject  # assuming the fetchRepoData method is in MyObject in mymodule.py

class TestMyObject(unittest.TestCase):
    def test_fetchRepoData(self):
        repo_path = "some/repo/path"
        mock_response = requests.Response()
        mock_response.status_code = 200
        mock_response._content = b'{"description": "desc", "html_url": "url", "pushed_at": "push_time", "stargazers_count": 10, "forks_count": 5}'

        my_obj = MyObject()
        my_obj.fetchRepoData(repo_path, mock_response)

        self.assertEqual(my_obj.data['repo_path'], repo_path)
        self.assertEqual(my_obj.data['description'], "desc")
        self.assertEqual(my_obj.data['html_url'], "url")
        self.assertEqual(my_obj.data['pushed_at'], "push_time")
        self.assertEqual(my_obj.data['stargazers_count'], 10)
        self.assertEqual(my_obj.data['forks_count'], 5)

if __name__ == '__main__':
    unittest.main()
```

## doctest

### Overview

[`doctest`](https://docs.python.org/3/library/doctest.html) is a unique testing framework in Python that turns interactive sessions into test cases. It's part of the standard library and is less powerful than `pytest` or `unittest`, but it can be very convenient for simple cases and for including tests directly in your documentation.

### How it Works and Technologies Used

`doctest` works by looking at the interactive examples in your docstrings. These examples should look like they've been typed directly into the Python interpreter. `doctest` will then run those statements and compare the output to the expected output you've included in the docstring.

You can run `doctest` tests through the command line interface, or you can integrate them into your `unittest` or `pytest` tests.

### Differentiating Factors

The main differentiating factor of `doctest` is its simplicity and integration with docstrings. This makes it a great tool for testing examples in documentation, or for very simple tests. However, it's less suitable for more complex tests or for tests that need setup or teardown code.

Each of these testing frameworks has its own strengths and weaknesses, and the best one to use can depend on your specific use case. `pytest` is generally the most powerful and flexible, `unittest` provides a familiar structure for people with experience in other languages, and `doctest` is great for testing documentation and for simple tests.

### Simple Example

A `doctest` test case could be as follows:

```python
def sum(a, b):
    """
    >>> sum(1, 2)
    3
    """
    return a + b
```

### Our Example

Since `doctest` works with docstrings, it's less applicable to this function which involves HTTP requests. `doctest` is better suited for simpler, pure functions. However, for the sake of this example, here's how you might use `doctest` to test a simpler function:

```python
class MyObject:
    def add_two_numbers(self, a, b):
        """
        Adds two numbers together.

        >>> obj = MyObject()
        >>> obj.add_two_numbers(2, 3)
        5
        """
        return a + b
```

You can then run `doctest` on this file with:

```bash
python -m doctest mymodule.py
```

If the test passes, `doctest` will output nothing. If it fails, it will output a message about the failed test.

## Comparing and Contrasting Python Testing Frameworks

That was quite a bit of information! Here's a brief comparison of these three Python testing frameworks:

* **pytest**:
  * **Pros**: Simple syntax, powerful, detailed error reports, rich ecosystem of plugins.
  * **Cons**: Not part of the standard library, can be overkill for very simple applications.
* **unittest**:
  * **Pros**: Part of Python's standard library, rich set of features, test discovery mechanism.
  * **Cons**: Verbose syntax, lacks some features provided by external libraries.
* **doctest**:
  * **Pros**: Tied closely with documentation, simple syntax, part of the standard library.
  * **Cons**: Not suitable for complex testing scenarios, does not support setup and teardown code.

## Additional Python Testing Tools

* **Coverage**: A tool for measuring code coverage of Python programs. It monitors your program and notes which parts of the code have been executed.
* **Mock**: A library for testing in Python. It allows you to replace parts of your system under test and make assertions about how they have been used.
* **Hypothesis**: A powerful, flexible, and easy-to-use library for property-based testing.

Additionally, I highly recommend utilizing an IDE for Python development, whether you're testing or not! IDEs like [VSCode](https://code.visualstudio.com/) and [IDLE](https://docs.python.org/3/library/idle.html) provide a dramatically improved development environment for everything from writing a simple web scrapper to creating the next Netflix! And when it comes to testing, IDEs provide several tools to make your life so much easier. If you're interested, perhaps we will cover this further in a future post!

## Conclusion

Testing is a vital aspect of any serious programming project, and Python provides an array of tools for efficient and effective testing. Understanding the features, strengths, and limitations of each framework can help you choose the right one for your project. Moreover, enhancing your testing with additional tools like `coverage`, `mock`, and `hypothesis`, as well as a properly configured IDE, can provide a more comprehensive testing environment for your Python applications.

If you are particularly interested in the idea of Testing and would like to improve your processes to include more tests, look into the below link book **Test Driven Development**. While an aging book, having been written in 2002, it still holds transformative concepts that every developer and architect ought to understand.

Until next time, that ends this stream of Meaty Bytes!

## References

1. [Test Driven Development: By Example](https://amzn.to/42Elcz2) by Kent Beck - authoritative book on the concept of Test Driven Development
2. [Right Python Framework Selection for Automation Testing](https://www.design-reuse.com/articles/53050/right-python-framework-selection-for-automation-testing.html) - Review of other popular Python Testing Frameworks
3. [Python's unittest documentation](https://docs.python.org/3/library/unittest.html)
4. [pytest documentation](https://docs.pytest.org/en/latest/)
5. [doctest documentation](https://docs.python.org/3/library/doctest.html)
6. [Coverage documentation](https://coverage.readthedocs.io/)
7. [Mock documentation](https://docs.python.org/3/library/unittest.mock.html)
8. [Hypothesis documentation](https://hypothesis.readthedocs.io/)
