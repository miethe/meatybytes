---
title: "Harnessing the Power of Python Virtual Environments and Beyond"
date: 2023-06-12
author: "Nick Miethe"
description: "An overview of Python 3 development best practices with the usage of virtual environments."
tags: ["Python", "Development", "Virtual Environments", "Containers", "OpenShift", "venv", "virtualenv", "pipenv", "Poetry", "pip"]
categories: ["Technical", "Introduction", "Guide"]
topics: ["Python", "Software Development"]
series: ["Virtually Py"]
series_order: 1
---

## Introduction

Welcome back to another stream of knowledge from none other than [MeatyBytes.io](/)! I'm your host and author, Nick Miethe, serial hobbyist and OpenShift Evangelist. Today, we're continuing down the path of Python development. And if you're wondering, don't worry, I'll be tying this series back to OCP in a few posts!

Mastering the art of Python development isn't just about writing great code. It's also about creating the ideal environment to facilitate that process. This post explores the realm of Python virtual environments and containerization tools to help you achieve a seamless development process. We will not only delve into the details of different Python virtual environments and example configurations, but also contrast these tools with running Python apps in containers. But first, let's clarify why these tools are important.

![](python-virtual-envs.webp "[Source](https://www.dataquest.io/blog/a-complete-guide-to-python-virtual-environments/) Example of virtual envs")

## Understanding the Need for Virtual Environments and Containers

When you're working on a Python project, it's not uncommon to have dependencies specific to that project. These dependencies may conflict with the packages installed on your system or other projects, and that's where virtual environments come in. Virtual environments allow you to isolate your Python setup on a project-by-project basis, hence mitigating the risk of dependency conflicts.

On the other hand, containers, such as those used in OpenShift, provide even greater isolation by encapsulating the entire runtime environment, including the application, its dependencies, and the system libraries and settings.

## The Tools of the Trade

## venv

`venv` is an in-built module in Python 3.3+ that provides support for creating lightweight, isolated Python environments. It's a simpler and more integrated alternative to `virtualenv` which was commonly used prior to `venv`.

### Example Configuration

```bash
python3 -m venv /path/to/new/virtual/environment
source /path/to/new/virtual/environment/bin/activate
```

### How it Works and Technologies Used

`venv` works by creating a directory that contains all the necessary executables to use the packages that a Python project would need. It essentially creates a self-contained environment with its own installation directories, isolating it from the system directory.

`venv` uses a script (`pyvenv.cfg`) to specify the settings of the virtual environment, including the Python version and the location of the base-prefix, base-exec-prefix, and base-executable.

When a `venv` environment is activated, the `PATH` environment variable is temporarily adjusted so that the Python interpreter and installed scripts associated with the environment are used first. This allows users to install and run packages without interfering with system-wide packages or other virtual environments.

### Differentiating Factors

The primary difference between `venv` and other virtual environment tools is its integration with Python itself. Since `venv` is bundled with **Python 3.3** and later, there's no need to install any additional packages to use it.

However, `venv` doesn't provide some advanced features provided by other tools, such as managing multiple environments simultaneously or handling dependencies across different environments.

## virtualenv

`virtualenv` is a very popular tool that creates isolated Python environments. It was developed before `venv` and has been widely used for creating multiple separate Python environments for different projects, each with their own dependencies. It's more flexible than `venv` as it allows creating environments for any Python version and can create environments for both Python 2 and 3.

### Example Configuration

```bash
pip install virtualenv
virtualenv venv
source venv/bin/activate
```

### How it Works and Technologies Used

`virtualenv` works similarly to `venv` by creating a directory that contains all the necessary executables to use the packages that a Python project would need.

One key difference is that `virtualenv` copies the Python interpreter into the virtual environment, whereas `venv` just changes the `PATH` to point to the system's Python interpreter.

The `virtualenv` command can be used with a variety of options, including specifying the Python interpreter version and the no-site-packages option to prevent the new virtual environment from inheriting packages from the global Python interpreter.

### Differentiating Factors

`virtualenv` is more feature-rich than `venv`. It allows for more customization and works with earlier versions of Python (< 3.3). It also provides an easy way to duplicate an existing environment with the same packages.

However, `virtualenv` is not integrated into Python and must be installed separately. It's also not as simple and straightforward to use as `venv`.

## pipenv

`pipenv` is a tool that aims to bring the best of all packaging worlds to the Python world, combining the functionalities of Pipfile, pip (Python's package manager), virtualenv, and a few other tools into one simple command-line tool. It's highly recommended if you need a high level of automation and simplicity.

### Example Configuration

```bash
pip install pipenv
pipenv install <package>
```

### How it Works and Technologies Used

`pipenv` creates and manages a virtual environment for your projects, and adds or removes packages from your `Pipfile` as you install/uninstall packages. It also generates the ever-important `Pipfile.lock`, which is used to produce deterministic builds and create a snapshot of your working environment.

When you run a `pipenv` command, `pipenv` will automatically use the virtual environment associated with the current project. This is determined by the location of the `Pipfile`.

### Differentiating Factors

`pipenv` is designed to simplify workflow process by providing a higher-level, all-in-one package management tool. It automatically manages a separate virtual environment for each project and even handles the creation and deletion of environments as needed.

`pipenv` also introduces `Pipfile` and `Pipfile.lock`, which are designed to replace the traditional `requirements.txt` in Python applications. These files provide more capabilities and can handle complex scenarios better than `requirements.txt`.

However, `pipenv` has been criticized for being slower than other tools and for having a more complex user interface.

## Poetry

`Poetry` is a Python packaging and dependency management tool. It was developed as a one-stop solution for Python project management, handling everything from dependency resolution to package publishing.

### Example Configuration

With Poetry, you will generally use a `pyproject.toml`, which is where dependencies are declared. See an example below:

```toml
[tool.poetry]
name = "my_project"
version = "0.1.0"
description = "My awesome Python project"
authors = ["Your Name <your.email@example.com>"]

[tool.poetry.dependencies]
python = "^3.8"
requests = "^2.25.1"

[tool.poetry.dev-dependencies]
pytest = "^6.2.2"

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"
```

In this configuration, `requests` is added as a main dependency and `pytest` is added as a development dependency.

To install these dependencies, you can use the `poetry install` command:

```bash
poetry install
```

This will create a new virtual environment (if one doesn't already exist), and it will install all the specified dependencies in the `pyproject.toml` file.

To add a new dependency to your project, you can use the `poetry add` command. Similarly, to add a development dependency, you can use the `--dev` flag. For example, to add the `numpy` package, plus the `black` code formatter as a development dependency, you'd run:

```bash
poetry new my_project
poetry add numpy
poetry add --dev black
```

This will update your `pyproject.toml `and `poetry.lock` files and install the `numpy` and `black` packages in your virtual environment.

### How it Works and Technologies Used

`Poetry` uses `pyproject.toml` to replace the traditional `setup.py`, `requirements.txt`, `setup.cfg`, `MANIFEST.in` and `Pipfile`.

When you run `poetry install`, Poetry creates a virtual environment, if one doesn't already exist, and installs the current project along with its dependencies.

`poetry.lock` file is created after the installation to lock down the exact versions of the dependencies. This is used to ensure that everyone working on the project will have the same versions of dependencies, leading to deterministic and repeatable builds.

### Differentiating Factors

`Poetry` simplifies dependency management. It automatically resolves and installs dependencies, and it can also check for security vulnerabilities and license information.

`Poetry` also supports semantic versioning, and it can resolve dependencies based on the versioning constraints.

The use of `pyproject.toml` is also a notable feature, as it provides a unified configuration file for Python projects, as recommended by **PEP 518**.

However, `Poetry` has been criticized for its steep learning curve, especially for those accustomed to the standard Python tooling such as `pip` and `setuptools`. Additionally, some users have reported slow performance and issues with complex dependency resolution.

## Comparisons

`venv` and `virtualenv` are both very similar, as they serve the same fundamental purpose: isolating project dependencies. `venv` is more streamlined and included with Python 3 by default, but `virtualenv` offers more flexibility in terms of Python version management. `pipenv` simplifies things further by combining package management and virtual environment management into a single tool. `Poetry` is also a very capable tool and perhaps most appropriately follows PEP guidelines, however it can be difficult to use correctly.

## Containers: The Next Level of Isolation

**Containers** take isolation to the next level by wrapping not just dependencies, but the entire runtime environment. This makes your Python application portable and ensures that it behaves the same way across different platforms. You might use containers when you want to virtualize your entire development system environment, versus just that specific Python project environment.

### Example Configuration

```dockerfile
FROM python:3.7
WORKDIR /app
COPY . /app
RUN pip install -r requirements.txt
CMD ["python","your_script.py"]
```

## Comparison with Python Virtual Environments

Python virtual environments, such as `venv`, `virtualenv`, `pipenv`, and `Poetry`, are tools specifically designed for Python applications. They create an isolated environment for Python applications, including a separate Python interpreter and Python libraries.

However, unlike containers, they do not isolate the application at the system level. They do not include system libraries or processes, and they do not isolate the file system or networking. This means that Python virtual environments are less isolated than containers, and the application may behave differently on different systems.

Python virtual environments are also simpler and less resource-intensive than containers. They do not require a separate system environment, and they can be created and managed using simple Python commands.

In terms of use cases, Python virtual environments are often used during the **development** process, when you need to manage dependencies for different Python projects. Containers, on the other hand, are often used for **deploying** applications, when you need to ensure that the application runs the same way on different systems. Alternatively, you might use a container if you need to virtualize your entire development system, for example to use cloud-based development environments.

In summary, while Python virtual environments and containers serve similar purposes, they are used in different scenarios. Python virtual environments are simpler and more suitable for managing Python dependencies during development, while containers provide a higher level of isolation and are more suitable for deploying applications.

## Which To Use

### venv

* Python 3.3+
* Great for beginners, or need for simple environments
* Great for CI/CD Pipelines and other Automation
* If need more advanced features, should probably use `pipenv` or `Poetry`

### virtualenv

* Python < 3.3
* Need slightly more customization that `venv`
* Some tools use `virtualenv` under the hood
* If using Python 3.3+ and don't need extra features, `venv` might be simpler

### pipenv

* Combines virtual environment management and dependency management into a single tool
* Especially useful for larger projects or collaboration
* Introduces `Pipfile` and `Pipfile.lock`, with more capabilities than `requirements.txt` for more complex scenarios
* Has been criticized for being slower and more complex than other tools

### Poetry

* Ideal for managing complex projects with many dependencies
* Especially useful for larger projects or libraries due to semantic versioning
* Good choice if you're planning to publish your package with *built-in support* for package publishing
* Has been criticized for slow performance and steep learning curve

## Conclusion

When it comes to Python development, having the right environment is crucial. Whether you opt for `venv`, `virtualenv`, `pipenv`, `Poetry`, or even containers entirely depends on your specific project requirements. As a Python developer, it's important to familiarize yourself with these tools and understand when to use which.

Remember, "A smooth sea never made a skilled sailor." Master these tools to navigate the choppy waters of Python development!

## References

1. [Python's guide on venv](https://docs.python.org/3/library/venv.html)
2. [Python's guide on virtualenv](https://virtualenv.pypa.io/en/latest/)
3. [Python's guide on pipenv](https://pipenv.pypa.io/en/latest/)
4. [Guide on Poetry](https://python-poetry.org/docs/basic-usage/)
5. [Three ways to deploy a Python app into an OpenShift cluster | IBM Developer](https://developer.ibm.com/tutorials/deploy-python-app-to-openshift-cluster-source-to-image/)
6. [Containerized Python Flask development on Red Hat OpenShift | Red Hat Developer](https://developers.redhat.com/blog/2019/02/18/containerized-python-flask-development-environment-red-hat-codeready-workspaces)
