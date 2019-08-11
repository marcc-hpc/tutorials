---
title: The R language
keywords: introduction, outline
last_updated: August 12, 2019
sidebar: shortcourse_sidebar
permalink: shortcourse_rlang.html
folder: shortcourse
toc: false
---

The `R` language is a very popular open-source language for statistical computing and visualization. We offer several versions of `R` which itself has access to an enormous library of packages from the [comprehensive R archive network](https://cran.r-project.org/mirrors.html) (CRAN). In this section we will show you how to install packages from CRAN on your account.

## Selecting an `R` version

Recall that the [modules system](shortcourse_modules.html) is [hierarchical](shortcourse_modules.html#hierarchical-modules) which means that the software stack uses only a single compiler at one time. Since `R` will often compile codes when you install packages, it is essential that you *keep track of which `R` version you are using*. For this tutorial we will use the default Intel compiler and `R` version `3.5.1`. Load these modules with the following commands.

~~~
$ ml restore
$ ml intel
$ ml R/3.5.1
~~~

You may see a warning that tells you where `R` will install packages.

~~~
MARCC Info: R is loaded,
'/home-4/rbradle8@jhu.edu/R/x86_64-pc-linux-gnu-library/3.5/intel/18.0' 
  is the default package location for user personal libraries; 
  we will attempt to create the package location if it does not exist. 
  Tired of this message? Type 'touch $HOME/.ignore_info' 
~~~

You are free to follow these instructions to suppress the message. Open the `R` terminal and check the library path. It exists inside your home directory, in a folder that specifies the `R` version and the compiler.

~~~
$ R

R version 3.5.1 (2018-07-02) -- "Feather Spray"
Copyright (C) 2018 The R Foundation for Statistical Computing
Platform: x86_64-pc-linux-gnu (64-bit)

R is free software and comes with ABSOLUTELY NO WARRANTY.
You are welcome to redistribute it under certain conditions.
Type 'license()' or 'licence()' for distribution details.

  Natural language support but running in an English locale

R is a collaborative project with many contributors.
Type 'contributors()' for more information and
'citation()' on how to cite R or R packages in publications.

Type 'demo()' for some demos, 'help()' for on-line help, or
'help.start()' for an HTML browser interface to help.
Type 'q()' to quit R.

> .libPaths()
[1] "/home-net/home-4/rbradle8@jhu.edu/R/x86_64-pc-linux-gnu-library/3.5/intel/18.0"
[2] "/software/apps/R/3.5.1/intel/18.0/lib64/R/library"
~~~

As promised from the warning message, your personalized packages will be written to `~/R/x86_64-pc-linux-gnu-library/3.5/intel/18.0`. We recommend that you monitor the disk usage on your home directory because there is a `20 GB` limit. 

It is also essential to keep track of the `R` version and compiler you are using. We recommend including this in your workflow documentation.

## Installing packages

Installing a custom set of packages is relatively simple. We recommend [using an interactive session](shortcourse_interact.html) to do this so that you do not disturb other users on the login nodes. In the following example we will install `ggplot2`.

~~~
$ R
> install.packages('devtools')
# select a CRAN mirror
# installation takes a few minutes
~~~

You will see a number of compile steps required to build this package. Afterwards you can use the package with the `library` command.

~~~
> library('devtools')
~~~

### Additional compile flags

You may need to use additional compile flags when building packages in `R`. Here is an example `Makevars` file to accomplish this.

~~~
$ cat ~/.R/Makevars
CC=icc -std=gnu11
CXX1XSTD = -std=c++0xi
CXX_STD = CXX11
~~~

We recommend consulting the [R documentation](https://cran.r-project.org/doc/manuals/r-release/R-exts.html#Using-Makevars) for more information.

### Visualization

If your analysis requires visualization, you have three options.

1. Use `X11` forwarding described in the [software prerequisites](shortcourse_prereq.html#tunneling-windows) section and the `ssh -X` or `ssh -Y` flags described in the [login](http://localhost:4000/shortcourse_login.html#graphical-interfaces) section.
2. Write images to disk and download them with `scp` using the method described in the [transferring files](shortcourse_transfer.html) section.
3. Use the [portal tools](https://www.marcc.jhu.edu/getting-started/interactive-development/).

### RStudio

If you are interested in graphical tools that provide R and Python in a web browser, see the [portal tools](https://www.marcc.jhu.edu/getting-started/interactive-development/) section of our website. 

<a class="btn btn-primary" href="shortcourse_containers.html">Next: containers</a>
