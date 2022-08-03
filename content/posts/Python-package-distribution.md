---
title: "Python application distribution [!published draft]"
date: 2022-07-08T12:11:41+02:00
draft: false
---

## Desired results
- You want to easily distribute and share your application with your users (including other developers) using python standard distribution format.
- You want your application to be installed using pip `pip install my-awesome-application`.
- You want to access your application in your other projects on your computer  `import my-awesome-application`.
- You want to be able to run your application from the terminal as a standalone from the terminal, without having to execute it via python and specifying the full path.
  ```zsh
  # you want:
  $ my_awesome_app
  # instead of:
  $ cd /my/project/path
  $ python my_awesome_app
  ```

<!-- incredients -->
## What you will need
- setuptool python package: its a python package used for managing the packaging & distribution process of python applications.
- Your python project (refer to this post to learn how to organise/structure you python application code).
- twine: needed if you are going to upload your application to pypi.

<!-- ## Steps -->
## setup.py
Create a file named setup.py at the root of your project folder as follows:
n.b. your project folder should not be a package itself.

```
project/
    ... # your project folders & files
    setup.py
```

The setup.py should call setup(...) function from setuptools package, with details about your application and how to package it as parameters.
it will look like the following:

```python
# setup.py
from setuptools import setup
setup(
    name="application_name",
    version="version"
)
```

### setup(...) function parameters
- name:
- version:

full list of parameters can be viewed here (link)

## Install your package locally
To install your application locally within your python environment, run following command:
`python install e .`
this installs your application locally, and hence can be accessed via import inside other python projects.

## Build your package for distribution
Python applications can be distributed as readable source file(s), or as binary:
this packages can be installed via pip
to build a source distribution, run:
`sdist ...` think of this as short for source-distribution
this create a folder ....

to build a binary distribution, run:
`bdist ...`

why you might choose one distribution over the other?

## Distribute your package

Once you have build your package (source or binary), it can be shared however, flash, email, telegram, version control, & most preferebly via pypi,
and your user can installed it like `pip install ...`

#### Distribute via pypi
To distribute via pypi, you will need to:
- Create an account on pypi.
- Upload your package to pypi using twine by running following command, twine will ask for some details to get to your account

Users can now access your package via pip, using name you define in setup.py
you can test this locally by running pip install your package

## Notes

steps tested on Ubuntu 20 LTS

## Refs
