---
title: Python and Virtual environments
keywords: introduction, outline
last_updated: August 12, 2019
sidebar: shortcourse_sidebar
permalink: shortcourse_python.html
folder: shortcourse
toc: true
---

Python is one of the most popular scripting languages for academic computation. We offer several different versions on *Blue Crab* along with three methods for extending with additional libraries. We will review these methods in this section.

## Python versions

Use the `module spider python` command to list the available versions.

~~~
python/2.7-anaconda
python/2.7-anaconda53
python/2.7
python/3.6-anaconda
python/3.6
python/3.7-anaconda
python/3.7-anaconda-2019.03
python/3.7
~~~

We recommend using the most recent version whenever possible.

## Python packages

Most python packages are distributed on the [Python package index](https://pypi.org/) and can be installed with a Python-based program called `pip`. Source packages can be uncompressed and installed with `python setup.py install --user` in concert with any of the three methods below.

In the following section we will install the popular [Numpy](https://www.numpy.org/) package using one of three different methods.

### 1. Direct installation

Load the Python 3.7 module and install the program directly with `pip install --user`. The `--user` flag will install the package to a hidden folder at `~/.local`. Python will check this folder for packages.

~~~
$ ml python/3.7
$ pip install --user numpy
$ python
>>> import numpy
>>> numpy.__file__
'/home-4/rbradle8@jhu.edu/.local/lib/python3.7/site-packages/numpy/__init__.py'
~~~

You can see that the numpy package is installed in your home directory. Note that these packages depend on your python version. If you switch Python versions with the `module` command, you may need to install the packages again.

If you receive a "permission denied" error when installing packages with `pip`, this is probably because you have forgotten the `--user` flag and do not have superuser permissions on your computer.

### 2. Virtual environments

A Python virtual environment provides a *sandbox* or self-contained environment where you can install packages directly. To use a virtual environment, you must first install it, and then activate it. Once you activate it, you will have full privileges to install software inside of it. Later you can deactivate the environment. ]

> Virtual environments are extremely useful for reproducing your workflow. You can carefully control and account for all software packages you have installed inside a virtual environment.

In the following example, we will create a virtual environment, activate it, and install `numpy`. The virtual environment will reside in a local folder called `venv`. Note that we use `./` to indicate "here" in BASH to emphasize that the argument is a relative path.

~~~
$ ml python/3.7
$ python -m venv ./venv
$ source ./venv/bin/activate
(venv) $ python
(venv) $ pip install numpy
>>> import numpy
>>> numpy.__file__
'/home-net/home-4/rbradle8@jhu.edu/venv/lib/python3.7/site-packages/numpy/__init__.py'
>>> exit()
(venv) $ deactivate
$ 
~~~

You will notice that your prompt changes when you have activated the environment to remind you. Virtual environments can be accessed from anywhere as long as you use the full path to activate them. You can use `pip freeze > requirements.txt` to make a snapshot of the packages installed in your virtual environment and later use that file to reconstitute the environment on another system with `pip install -r requirements.txt`. 

### 3. Anaconda

[Anaconda](https://www.anaconda.com/distribution/) is a general-purpose package manager that offers many Python packages but also includes `R` packages and additional programs that are sometimes difficult to install from source, or without superuser access to the operating system's package manager.

We supply a recent copy of Anaconda from which you can install many other packages. We also provide a set of built-in anaconda environments. We use Anaconda with the `conda` command. In the following example we use `conda activate` to access a pre-installed environment with [qiime2](https://qiime2.org/).

~~~
$ module load anaconda/2019.03
$ conda env list
# conda environments:
#
base                  *  /software/apps/anaconda/2019.03
macs2-rsem               /software/apps/anaconda/2019.03/envs/macs2-rsem
nglview                  /software/apps/anaconda/2019.03/envs/nglview
plotly                   /software/apps/anaconda/2019.03/envs/plotly
qiime2-2018.8            /software/apps/anaconda/2019.03/envs/qiime2-2018.8
tensorflow-1.12.2        /software/apps/anaconda/2019.03/envs/tensorflow-1.12.2
torchvision              /software/apps/anaconda/2019.03/envs/torchvision
$ conda activate qiime2-2018.8
(qiime2-2018.8) $ python --version
(qiime2-2018.8) $ python
>>> import qiiime2
~~~

It is also possible to make a virtual environment in much the same was as the method above.

~~~
$ conda create -p ./conda_env
Collecting package metadata: done
Solving environment: done

==> WARNING: A newer version of conda exists. <==
  current version: 4.6.14
  latest version: 4.7.11

Please update conda by running

    $ conda update -n base -c defaults conda

## Package Plan ##

  environment location: /home-net/home-4/rbradle8@jhu.edu/conda_env

Proceed ([y]/n)? y

Preparing transaction: done
Verifying transaction: done
Executing transaction: done
#
# To activate this environment, use
#
#     $ conda activate /home-net/home-4/rbradle8@jhu.edu/conda_env
#
# To deactivate an active environment, use
#
#     $ conda deactivate

$ conda activate ./conda_env
(conda_env) $ pip install numpy
(conda_env) $ conda install -c bioconda bowtie
(conda_env) $ conda deactivate
$
~~~

Remember to use `conda deactivate` to leave the environment when you are done. In the example above we can use both `pip` and `conda` to install packages. The [Anaconda Cloud](https://anaconda.org/) contains a searchable list of packages and associated channels (e.g. `bioconda`) which provide these packages.

Note that you can use `conda create -p ./conda_env` to use an absolute path for your environment or use `conda create -n conda_env` to use a name instead. When you use a name, the environment is installed in the home directory. Please be mindful of your quota when you install the package with a name. We recommend using an absolute path to keep track of things.

As with the virtual environments, you can also install a conda environment from a single file called a *manifest*. Create a file called `reqs.yaml` with the following contents.

~~~ yaml
# reqs.yaml
dependencies:
  - numpy
  - bioconda::bowtie
  - pip
  - pip:
    - sphinx
~~~

You can create an environment with these packages with the following method.

~~~
$ ml anaconda/2019.03
$ conda env create --file reqs.yaml -p ./custom_env
~~~

Using a requirements file provides a paper trail which is useful for reproducing your work and sharing it with others.

## Which method is best?

If you only need to install a few packages, `pip install --user` is the easiest method. Python virtual environments should be used for even moderately complex sets of packages because it is only marginally more difficult. Anaconda should be used when you require very complicated packages only available on Anaconda Cloud, or you need extra packages to be installed with `R` as well.

## Portal tools

If you are interested in portal tools that provide R and Python in a web browser, see the [portal tools](https://www.marcc.jhu.edu/getting-started/interactive-development/) section of our website.

<a class="btn btn-primary" href="shortcourse_rlang.html">Next: the R language</a>
