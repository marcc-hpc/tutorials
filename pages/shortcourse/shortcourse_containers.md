---
title: Containers
keywords: introduction, outline
last_updated: August 12, 2019
sidebar: shortcourse_sidebar
permalink: shortcourse_containers.html
folder: shortcourse
toc: false
---

*Containers* are abstractions that organize a self-contained set of files into a single entity that acts nearly independently from the rest of the machine. The design of most containers extends the idea of a virtual environment to its logical conclusion by providing an isolated environment with an entire interdependent software stack. In short: a container is a *computer within a computer*.

## Singularity

The most popular containerization program is [Docker](https://www.docker.com/). Docker is designed for industry use, and therefore has a security model which is incompatible with multi-tenant HPC resources where users are denied superuser access.

[Singularity](https://sylabs.io/docs/) is a platform for using containers, including Docker containers, on multi-tenant systems safely. *Blue Crab* currently offers

### Singularity modules

Several items on the `module` list are already provided by Singularity for convenience. For example, the module that provides Tensorflow 1.10 with Keras has a `(c)` tag on the `module avail` list indicating that it uses Singularity. 

~~~
$ module load tensorflow/1.10.1-gpu-py3
$ type tensorflow
$ type python
python is a function
python () 
{ 
    singularity exec --nv /software/apps/tensorflow/1.10.1-gpu-py3/tensorflow-1.10.1-gpu-py3.simg python3 "$@"
}
$ python
>>> import tensorflow as tf
>>> tf.__version__
'1.10.1'
>>> tf.__file__
'/usr/local/lib/python3.5/dist-packages/tensorflow/__init__.py'
~~~

The `type` command indicates that the python command is now a BASH function which calls `singularity exec` and sends the rest of the arguments through to the Python that exists inside the image. Therefore, without knowing it, you might be using a container already.

Singularity provides direct access to your storage devices, so you can automatically access files from the host system from inside the container.

When we imported tensorflow above, and checked the `__file__`, we find the location of the file *inside the container*. This file is absent from the host system. Understanding the distinction between inside and outside the container is essential.

### Installing containers

One of the most flexible ways to deploy new codes on *Blue Crab* is to use Singularity to retrieve an image from an external source using the `singularity pull` command. In this example we can retrieve the latest Julia version.

~~~
$ singularity pull docker://julia:1.1.1 julia-1.1.1.simg
$ singularity run julia
$ singularity run julia-1.1.1.simg 
               _
   _       _ _(_)_     |  Documentation: https://docs.julialang.org
  (_)     | (_) (_)    |
   _ _   _| |_  __ _   |  Type "?" for help, "]?" for Pkg help.
  | | | | | | |/ _` |  |
  | | |_| | | | (_| |  |  Version 1.1.1 (2019-05-16)
 _/ |\__'_|_|_|\__'_|  |  Official https://julialang.org/ release
|__/                   |

julia>
~~~

When using Singularity, it is best to set the temporary directory to your scratch space.

~~~
$ mkdir ~/scratch/scache
$ export SINGULARITY_TMPDIR=~/scratch/scache
$ export SINGULARITY_LOCALCACHEDIR=~/scratch/scache
~~~

For more details on using Singularity, see the [extensive documentation](https://sylabs.io/docs/).

## Custom containers

It is possible to build a custom container with a remote build service, or by installing Singularity on your local machine. Ask the instructor for more details if you are interested in this option. When *Blue Crab* upgrades to Singularity 3, we will offer instructions for doing this automatically. Please see our [extra documentation](https://marcc-hpc.github.io/esc/common/singularity) for more details.

## Conclusion of the short course

> This concludes the short course. We hope that you have found the exposition of scientific computing topics and our description of our hardware, software, and system to be helpful for your work.

{% include important.html content='Extra documentation and guides can be found at <code><a href="https://marcc-hpc.github.io/esc/">marcc-hpc.github.io/esc</a></code>.' %}
