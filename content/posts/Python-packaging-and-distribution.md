---
title: "Python packaging and distribution"
date: 2022-07-08T12:11:41+02:00
draft: true
---

Python applications are written inside \*.py files referred to as modules. This can further be organized inside packages (container of modules), which are simply folders that aside your modules, must contain a special python file with the name '__init__.py'. this file can just be blank, & your package can just have only this file with your code in there - if its not runnable ofcourse (confirm).
this is so that your code can be accessible to other part or your code, or other project entirely via import.

further configuration must be put in place to ensure your package can installed via pip, & also make it easily sharable with other developers, either by simply sending them an instable packaged project  -say via flash, email, telegram or some other file sharing means, or most commonly "if your project is open source" via pypi (python package index). u can ofcourse just share your application directly as files that are run directly without installation via pip if you choose.

setuputils python package is used for managing the packaging & distribution process of python applications. it requires some config files. the main & minimal config file being setup.py & also running some build commands!

the setup.py sits at the root of your project folder as follows:

```
project/
 pkg/...
 setup.py
```

ur project can have multple packages & whatever nesting of packages you desire, but it is recommended that you do not put any modules that form part of your application logic in your project's root folder, basicall have all of your code sitting in a package - meaning your project folder is not a package itself.

# setup.py

the setup.py at minimal, should call the `setup()` function from setuputils package, with details about your application & how to package it as input (to the setup function)
this function also converts setup.py into & script that can be run with some arguments for building your package.
it will look as follows:

```python
# setup.py
from setuputils import setup
pkg_configs = (...)
setup(pkg_config)
```

setup config options (setup fx input -- lookup in fx definition)

- name:
- version:
- ...

following is an example of a more thorough setup.py file

```python
#setup.py
...
```

as mentioned, your setup turns your application into an installable & provide you with commands for building & installing your application, some common option and build commands

local install
setup locally so your application is accessible within your python environment
run `python install e .`  this installs locally

for distribution
source distribution - ability to view code by people you share with (distribute to)
run 'sdist ...' this is short for source-distribute

for binary distribution, run 'bdist...' binary of package is called a wheel

once you've build you distributable (source or binary), this can be shared however, flash, email, telegram, version control, & simply installed via pip by the people you share with

to distribute (share) the package on pypi, you'd need to go through the following steps:

1. create an account on pypi
2. upload your package to pypi using twine by running following command. ,`twine will ask for some details to get to your account`

users can now access your package via pip, using name you defined in setup.py

Refs:
