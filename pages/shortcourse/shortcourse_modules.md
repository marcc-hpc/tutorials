---
title: Software modules
permalink: shortcourse_modules.html
sidebar: shortcourse_sidebar
folder: shortcourse
toc: false
---

We use an environment system called [Lmod](https://lmod.readthedocs.io/en/latest/) to serve software to all users. Advanced users who wish to install and compile their own software are free to do so as long as they do not require root access. All users are free to use the software modules.

## Software modules

When you log on to *Blue Crab*, you will already have access to several default modules.

~~~
$ module list

Currently Loaded Modules:
  1) centos7/current   2) intel/18.0   3) openmpi/3.1   4) MARCC/summer-2018
~~~

We include a `centos7` and `MARCC` module to provide supporting programs like `interact` and the standard tools foudn in the [CentOS](https://www.centos.org/) operating system. We also load the Intel compiler and OpenMPI automatically.

To see the available software, use the `module avail` command. You can always use the word `ml` instad of `module` to save time.

![available modules](figs/snap-module-avail.png)

This list is a menu of software that you are free to load and unload. When you load a module, we add the location of its associated programs to your `$PATH` so you can have access to them.

~~~
$ ml bowtie
$ bowtie --version
bowtie version 1.1.1
64-bit
Built on localhost.localdomain
Tue Sep 30 15:43:08 EDT 2014
Compiler: gcc version 4.1.2 20080704 (Red Hat 4.1.2-54)
Options: -O3 -m64  -Wl,--hash-style=both -DPOPCNT_CAPABILITY  
Sizeof {int, long, long long, void*, size_t, off_t}: {4, 8, 8, 8, 8, 8}
~~~

Checking the version confirms that we have access to the program.

### Searching by keyword

You can search for modules with the keyword command.

~~~
$ ml keyword pyth
----------------------------------------------------------------------------

The following modules match your search criteria: "pyth"
----------------------------------------------------------------------------

  biopython: biopython/1.72-py2, biopython/1.72-py3

  python: python/2.7-anaconda, python/2.7-anaconda53, python/2.7, ...

----------------------------------------------------------------------------

To learn more about a package execute:

   $ module spider Foo

where "Foo" is the name of a module.

To find detailed information about a particular package you
must specify the version if there is more than one version:

   $ module spider Foo/11.1

----------------------------------------------------------------------------
~~~

Since the module list cab be quite extensive, it is best to use both `keyword` and `spider` to search for them.

### Using spider to load modules

The `module spider` command is the best way to find more information about a program. Let's take a common example, program, [LAMMPS](https://lammps.sandia.gov/). If we search for LAMMPS we can see that there are several versions.

~~~
$ ml spider LAMMPS

----------------------------------------------------------------------------
  lammps:
----------------------------------------------------------------------------
     Versions:
        lammps/2016-ICMS
        lammps/20180822-gpu
        lammps/20180822
        lammps/20190208
        lammps/20190329
        lammps/20190514

----------------------------------------------------------------------------
  For detailed information about a specific "lammps" module (including how to load the modules) use the module's full name.
  For example:

     $ module spider lammps/20180822
----------------------------------------------------------------------------
~~~

If we search for a specific version, we receive instructions for loading it.

~~~
$ ml spider LAMMPS/20190208

----------------------------------------------------------------------------
  lammps: lammps/20190208
----------------------------------------------------------------------------

    You will need to load all module(s) on any one of the lines below before the "lammps/20190208" module is available to load.

      gcc/5.5.0  openmpi/3.1
~~~

### Module prerequisites

This particular copy of LAMMPS depends on a specific compiler and openmpi implementation. To load it, you must load the required modules first.

~~~
$ module load gcc/5.5.0
$ module load openmpi/3.1
$ module load lammps/20190208
~~~

You can sometimes omit the version number to use the latest or default version, however we recommend keeping track of the versions explicitly. Loading a core module before obtaining access to a software module such as `lammps/20190208` is a feature of the hierarchical module system.

### Hierarchical modules

The `module avail` results above included three sections:

1. Core Modules
2. Base Apps
3. Intel Compiler Dependent Apps

The Core Modules and Base Apps do not change. The Base Apps have been compiled against the system's default compiler. The core modules provide additional compilers or MPI implementations which hide or reveal dependent applications in the final section, in this case, the Intel compiler-dependent apps. When you change to a different core module, the dependent apps will change to reflect the available software.

This system is designed to prevent conflicts between dynamically linked libraries. Much of the supporting software stack is dynamically linked, which means that a particular piece of code asks for a function or symbol at run time, and locates that code in another file thanks to a set of environment variables. 

> We require that each software package has a uniform set of dependencies, compiled with a single compiler and MPI implementation, if necessary. This guarantees that our software is robust and internally consistent.

In the `lammps` example above, we had to load the gcc compiler before we could access the software because the Intel compiler was loaded by default, and we do not allow multiple compilers to be used at once.

### Switching compilers

Because we offer two compilers (gcc and Intel) and two main MPI implementations (OpenMPI and IntelMPI), we often have multiple versions of the same software pacakages. When you switch compilers or MPI implementations, Lmod will automatically find the equivalent software package.

Restore the default module commands, and then load the `R` module. If we switch from the default Intel compiler to the gcc compiler, Lmod will reload the corresponding `R` module for us. Remember that `ml` is equivalent to the `module` command. Running `ml` without an argument is equivalent to `ml list`.

~~~
$ ml restore
Resetting modules to system default

Due to MODULEPATH changes, the following have been reloaded:
  1) openmpi/3.1

$ ml R
$ ml --terse
openmpi/3.1
MARCC/summer-2018
centos7/current
intel/18.0
R/3.5.1
$ which R
/software/apps/R/3.5.1/intel/18.0/bin/R
$ ml gcc

Lmod is automatically replacing "intel/18.0" with "gcc/5.5.0".

Due to MODULEPATH changes, the following have been reloaded:
  1) R/3.5.1     2) openmpi/3.1

$ ml --terse
MARCC/summer-2018
centos7/current
gcc/5.5.0
R/3.5.1
openmpi/3.1
$ which R
/software/apps/R/3.5.1/gcc/5.5/bin/R
~~~

Later we will learn how `R` manages its own internal packages. Since these are often compiled, it is essential to keep track of your compiler in order to keep track of your compiled packages.

### Saving module collections

If you wish to create collections of related modules, you can load them and use `module save` to save them for later. You can load the group together with the `module restore` command.

~~~
$ ml restore
$ ml R/3.5.1
$ ml python/3.7
$ ml gnuplot/4.6.2
$ ml haskell/7.6.3
$ ml save omics_project_modules
Saved current collection of modules to: "omics_project_modules"
$ ml restore
$ ml --terse
centos7/current
intel/18.0
openmpi/3.1
MARCC/summer-2018
$ ml restore omics_project_modules
$ ml --terse
openmpi/3.1
MARCC/summer-2018
intel/18.0
R/3.5.1
centos7/current
python/3.7
gnuplot/4.6.2
haskell/7.6.3
~~~

If you run `module restore` without an argument it will load the system defaults. If you use `module save` without an argument, you will override the defaults for your account. The next time you log in, your modules will be the same as they were when you raun `module save`. We recommend using named collections for clarity.

You can use `ml purge` to remove all modules, but this will cause many important programs to disappear. 

### Private modules

The MARCC staff maintains the modules system, however you are free to extend it yourself. Let's create a module which gives us access to a folder of custom scripts we wish to use frequently. In the following example we will create a `bin` folder and our own private module tree. 

~~~
$ mkdir bin
$ touch bin/myscript
$ chmod u+x bin/myscript
$ vi bin/myscript # make a simple script here
~~~

Use the editor to make a "Hello, World!" script. You can imagine creating many useful scripts which you wish to be accessible from any path. Next we will make a private module tree with a Lua file. The [Lua](https://www.lua.org/) language is used by Lmod.

~~~
$ mkdir privatemodules
$ vi privatemodules/custom_tools.lua
~~~

Inside this file, we will prepend to our path. Add the following text to the file and then exit.

~~~
prepend_path("PATH","~/bin")
~~~

Now that we have created a module tree, we will tell Lmod to use it and then load our module.

~~~
ml use.own
ml av
~~~

The `ml av` list now contains a section with the path to `~/privatemodules` and a module called `custom_tools`. If you load it, you can access `myscript`.

~~~
$ ml custom_tools
$ myscript
Hello, World!
~~~

The `~/privatemodules` method described above allows you to compile and install any software to a custom location and then easily access it with a 

{% include note.html content="Separating the software dependencies from your workflow and using the module system to select your software is essential for developing reproducible, portable workflows." %}

<a class="btn btn-primary" href="shortcourse_python.html">Next: python</a>