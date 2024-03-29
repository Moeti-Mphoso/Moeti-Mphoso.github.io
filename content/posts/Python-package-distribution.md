---
title: "Python application distribution [!published draft]"
date: 2022-07-08T12:11:41+02:00
draft: false
---

## Desired results
- You want to easily distribute and share your application with your users (including other developers).
- You want your application to be installed via pip.
  ```zsh
  pip install my-awesome-application
  ```
- You want to access your application in your other projects on your computer.
  ```zsh
  import my-awesome-application
  ```
- You want to be able to run your application from the terminal as standalone, without having to execute it via python and specifying  or  navigating to your project folder.
  ```zsh
  # you want:
  $ my_awesome_app
  # instead of:
  $ cd /my/project/path
  $ python my_awesome_app.py
  # or
  $ python /my/project/path/my_awesome_app.py
  ```

<!-- incredients -->
## What you will need

- [setuptools](<https://pypi.org/project/setuptools/>): its a python package used for managing the packaging & distribution of python applications.
- Your python project (refer to [this post](../python-packaging-and-distribution) to learn how to organise your python application code).
- [wheel](): a package for compiling python code to a distributable binary format.
- [twine](https://pypi.org/project/twine/): a utility for publishing python packages on the [Python Package Index (PyPI)](https://pypi.org/), needed if you are going to upload your application to PyPI.

<!-- ## Steps -->
## setup.py
<!-- project metadata specification: this can be placed in setup(), setup.cnf or pyproject.toml -->
Create a file named setup.py at the root of your project folder.

```
project/
    ... # your project sub-folders & files
    setup.py
# n.b. your project folder should not be a package itself.
```

The setup.py file should call setup(...) function from setuptools package, with details about your application and how to package it as parameters.
The following is a setup.py code of [pydbml](https://github.com/Vanderhoof/PyDBML) project, note that you can put any other code you want in your setup.py file. (pydbml code might have been updated since writting this post).

```python
# setup.py
from setuptools import setup

SHORT_DESCRIPTION = 'Python parser and builder for DBML'

try:
    with open('README.md', encoding='utf8') as readme:
        LONG_DESCRIPTION = readme.read()

except FileNotFoundError:
    LONG_DESCRIPTION = SHORT_DESCRIPTION


setup(
    name='pydbml',
    python_requires='>=3.8',
    description=SHORT_DESCRIPTION,
    long_description=LONG_DESCRIPTION,
    long_description_content_type='text/markdown',
    version='1.0.1',
    author='Daniil Minukhin',
    author_email='ddddsa@gmail.com',
    url='https://github.com/Vanderhoof/PyDBML',
    packages=['pydbml', 'pydbml.classes', 'pydbml.definitions', 'pydbml.parser'],
    license='MIT',
    platforms='any',
    install_requires=[
        'pyparsing>=2.4.7',
    ],
    classifiers=[
        "Development Status :: 4 - Beta",
        "Environment :: Console",
        "Intended Audience :: Developers",
        "License :: OSI Approved :: MIT License",
        "Operating System :: OS Independent",
        "Programming Language :: Python",
        "Topic :: Documentation",
        "Topic :: Text Processing :: Markup",
        "Topic :: Utilities",
    ]
)
```


### setup(...) function parameters
- name: this is the distribution name of your application. (this is the only required parameter)
- version: your application version.
- author: author of the application, you!
- description: short summary of your application.
- long_description: detailed description of your application.
- package_dir: where packages are located, e.g under src folder `package_dir={'': 'src'}`
- packages: list of packages available in project, e.g find_packages('src')
- license:
- platforms:
- install_requires:
- classifiers:
- entry_points:
- extras: capability to include extra dependencies during installation


#### including data files; MANIFEST.in

#### setup.cfg?
#### pyproject.toml?
Metadata and configuration supplied via setup() is complementary to (and may be overwritten by) the information present in setup.cfg and pyproject.toml.
setup() is an older approach.

Links to setup.py files of some of my favorite python packages:
- [visi-data](https://github.com/saulpw/visidata/blob/develop/setup.py)
- [pgcli](https://github.com/dbcli/pgcli/blob/main/setup.py)
- [requests](https://github.com/psf/requests/blob/main/setup.py)
- [flask](https://github.com/pallets/flask/blob/main/setup.py)
- [pandas](https://github.com/pandas-dev/pandas/blob/main/setup.py)
- [ipython](https://github.com/ipython/ipython/blob/master/setup.py)

The full list of setup(...) function parameters can be viewed [here](pythonsetupparameters).

## Install your package locally (development mode)
To install your application locally, run the following command from your project folder:
```zsh
# python setup.py install e . # prior pip v21.1
pip install --editable .
```
This installs your application in your python environment, and hence can be imported inside any other python project within the same environment.
the `--editable` option means that a symbolic link to your application is created inside python's installation directory (side-packages), so that
any edits you make to your application can be reflected without having to re-install your application.

## Build your package for distribution
Python applications can be distributed as readable source file(s)/archives, or as pre-build binary archives (called wheels).
!python -m build

to build a source distribution, run:
`python setup.py sdist` think of this as short for source-distribution
this create a folder ....

to build a binary distribution, run:
`python setup.py sdist bdist_wheel`


above creates 3 folders:
- ./build
- ./<pkg_name>.egg-info
- ./dist

distributable package files are stored in ./dist folder

why you might choose one distribution over the other?

## Distribute your package

Once you have build your package (source or binary), it can be shared however way you want; email, telegram, version control, & most preferebly via pypi,
and your user can installed it like `pip install ...`

### distribute via cvs (gitlab & github)

Aside code repositories for hosting version controlled code, both gitlab & github provide package registries where your applications can be hosted and downloaded or installed from for use,
think pypi, apt or npm repositories.

the following instructions go through the steps required to distribute your python code on these registries.

-> registr, repository, potato, potato!
#### github package registry

Setup authentication required to publish your code, this is separate from the username & passport, ssh key or whatever you use to authenticate on gitlab so as to push your code to the repository.
Creating Personal Access Token with scope set to `api`(<https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html>)

repository = <https://gitlab.example.com/api/v4/projects/<project_id>/packages/pypi>
<https://gitlab.hisp.org/api/v4/projects/150/packages/pypi>

project_id:
 - project's URL-encoded path
 - project's ID (150 for dhis2api prefered, id autogenerated during project creation and displayed in project homepage)

version string validation (<https://docs.gitlab.com/ee/user/packages/pypi_repository/index.html#ensure-your-version-string-is-valid>)

Publish a PyPI package by using twine

`python -m twine upload --repository-url https://gitlab.hisp.org/api/v4/projects/150/packages/pypi dist/*`
twine will request username and password, provide token name for username and token for password

published package visible in Project's Packages and registries page, page also provides pip install command you'll use when installing the package.

# Install package

U'll need an access token since this is a private package - this is separate from the one setup above for publishing (ofcourse you can set a single token with the necessary scopes)
u can use a personal access token with scope set to `read_api`

`pip install dhis2api --index-url https://<token_name>:<token>@gitlab.hisp.org/api/v4/projects/150/packages/pypi/simple`
`pip install dhis2api --index-url https://packages_access:glpat-18Ft_PQ6yBdw86TCG3b_@gitlab.hisp.org/api/v4/projects/150/packages/pypi/simple`

refs:

- <https://docs.gitlab.com/ee/user/packages/pypi_repository/index.html>
- <https://docs.gitlab.com/ee/user/profile/personal_access_tokens.html>


refs: 

- <https://docs.gitlab.com/ee/user/packages/pypi_repository/index.html>
- <https://docs.gitlab.com/ee/user/packages/package_registry/index.html>
- <https://docs.gitlab.com/ee/user/project/deploy_tokens/#deploy-tokens>
- <https://www.revsys.com/tidbits/using-private-packages-python/>


### installing from gitlab package registry:
e.g `pip install "git+ssh://git@gitlab.hisp.org:722/Moetim/dhis2api.git#egg=dhis2api"`
pip install "git+ssh://git@gitlab.hisp.org:722/Moetim/dhis2api.git#egg=dhis2api&subdirectory=pkg_dir" # when package not in root of repo

#### Distribute via pypi
To distribute via pypi, you will need to:
- Create an account on pypi.
- Upload your package to pypi using twine by running following command, twine will ask for some details to get to your account

```zsh
 python -m twine upload --repository testpypi dist/*
```

Users can now access your package via pip, using name you define in setup.py
you can test this locally by running pip install your package

## Notes
- For help choosing an open source license, go to  <https://choosealicense.com/>
- You can test your distribution on [TestPyPI]() first before uploading to PYPI.

## Refs
<https://packaging.python.org/en/latest/>
<https://setuptools.pypa.io/en/latest/index.html>
<https://setuptools.pypa.io/en/latest/userguide/declarative_config.html>
vcs repos: <https://pip.pypa.io/en/stable/topics/vcs-support/>
https://pycon.switowski.com/02-packages/pipx/
