---
title: Software modules
permalink: shortcourse_bash.html
sidebar: shortcourse_sidebar
folder: shortcourse
toc: false
---

Providing software to many hundreds of users is not trivial, particularly given the complexity of many modern codes. To manage the complexity on a highly-heterogeneous, multi-tenant HPC cluster, we use an environment module system called [Lmod](https://lmod.readthedocs.io/en/latest/). Before we explain the modules system [in the next section](shortcourse_modules.html), it is essential to understand the shell environment.

## The BASH environment

Before we explain the modules system, it is essential to understand the BASH namespace. In the [login](shortcourse_login.html) we alluded to the use of BASH variables like `$USER` and in the [SLURM](shortcourse_slurm.html) section we noticed that SLURM sets some variables for us, like `$SLURM_JOB_ID`. In this section we will see how these variables are used to create a rich, flexible software environment.

> A brief note on terminology: as with *core*, and *parallel*, the word *environment* is overloaded and carries multiple meanings. For our purposes, an environment is an instance of a shell, typically a BASH shell, which carries a set of variables.

### Tab (auto) completion

Many linux systems offer a lazy way to run the `ls` command to list files. If you start to type an `ls` command with a target location, you can hit the <kbd>tab</kbd> key twice to see a list of ways to complete the phrase. This helps you see the contents of a directory without running a separate command.

The method works equally well with BASH variables. Type a `$` and then <kbd>tab</kbd> <kbd>tab</kbd> to see all of the possible variables. It may ask you to confirm that you want to see all of them in a `more` format (remember: use <kbd>q</kbd> to exit early).

### Inspecting the environment

There are more than a few interesting variables here. Note that I will use the hash (`#`) symbol inline with some commands to explain the right keystrokes. Note also that most `$` signs in this section are the prefix for a BASH variable, however I will continue the convention of using a `$` followed by a space to denote the command prompt, which precedes a command that you can try.

~~~
$ echo $ # use double-tabs here. anything following a hash is a comment
Display all 148 possibilities? (y or n)
$_                                 $__LMOD_REF_COUNT_PKG_CONFIG_PATH
$BASH                              $LMOD_SETTARG_FULL_SUPPORT
$BASH_ALIASES                      $__LMOD_STACK_CC
$BASH_ARGC                         $__LMOD_STACK_CXX
$BASH_ARGV                         $__LMOD_STACK_F77
$BASH_CMDS                         $__LMOD_STACK_F90
$BASH_COMMAND                      $__LMOD_STACK_FC
$BASH_ENV                          $LMOD_sys
$BASH_LINENO                       $LMOD_SYSTEM_DEFAULT_MODULES
$BASHOPTS                          $LMOD_VERSION
$BASHPID                           $LOADEDMODULES
$BASH_SOURCE                       $LOGNAME
$BASH_SUBSHELL                     $LS_COLORS
$BASH_VERSINFO                     $MACHTYPE
$BASH_VERSION                      $MAIL
$CC                                $MAILCHECK
$CLCK_ROOT                         $MANPATH
$colors                            $MARCC_COMPILER
$COLUMNS                           $MARCC_COMPILER_MAJOR
$COMP_WORDBREAKS                   $MARCC_COMPILER_MINOR
$CPATH                             $MKLROOT
~~~

The environment is really a *dictionary* of key-value pairs. To see the entire environment, try the `env` command.

We can view the values of different variables using `echo`.

~~~
$ echo $CC
icc
$ echo $PWD
/home-4/rbradle8@jhu.edu
$ pwd
/home-4/rbradle8@jhu.edu
$ echo $UID
3279
~~~ 

We can also set variables directly in the shell with an equals sign. We must avoid spaces.

~~~
$ PI="3.14159"
$ echo $PI
3.14159
$ DATA_SOURCE=~/data/experiment_results
~~~

By convention, we use uppercase letters for BASH variables, but you are also free to use lowercase letters. As with most programming languages, you cannot start a variable name with a number, use spaces and wildcard operators (`*`), and so on.

{% include note.html content="Setting a BASH variable is often a useful way to make your workflows more flexible because it's much easier to change a single value than to keep track of multiple \"hard-coded\" variables in your scripts." %}

## Paths

The single most important BASH variable is probably the `$PATH` variable. Whenever you run a program, the shell searches very location in the `$PATH` variable for a program of the same name. It gives you the first result, if any. When you run a specific program like `scratchquota`, the shell searches each colon-separated path in `$PATH` and returns the location of the program. You can use the `which` command to return the location of a program in the `$PATH`.

~~~
$ echo $PATH
/home-4/rbradle8@jhu.edu/bin:/software/apps/mpi/openmpi/3.1/intel/18.0/bin:/software/apps/compilers/intel/itac/2018.3.022/intel64/bin:/software/apps/compilers/intel/clck/2018.3/bin/intel64:/software/apps/compilers/intel/compilers_and_libraries_2018.3.222/linux/bin/intel64:/software/apps/compilers/intel/compilers_and_libraries_2018.3.222/linux/bin:/software/apps/marcc/bin:/software/centos7/bin:/software/centos7/sbin:/software/apps/slurm/current/sbin:/software/apps/slurm/current/bin:/usr/local/bin:/usr/bin:/usr/local/sbin:/usr/sbin:/software/apps/compilers/intel/parallel_studio_xe_2018.3.051/bin:/home-4/rbradle8@jhu.edu/bin
$ which scratchquota
/software/apps/marcc/bin/scratchquota
~~~

By controlling the contents of `$PATH`, we can reveal and hide different software packages. When we [introduced the hashbang](shortcourse_text.html#hashbang-or-what-is-a-program), we recommended using `#!/usr/bin/env python` to access Python. This is merely a programmatic way to signal that a script should be interpreted by a program that should be found somewhere in the environment, by searching the path. 

When you run `env python`, the shell finds and executes the `python` program, hence the command `/usr/bin/env python` is identical to running `python`. We could use `which python` to find the exact location, and use either of the following hashbangs in a script.

~~~
#!/software/centos7/bin/python
#!/usr/bin/env python
~~~

The second hashbang above is better, because it asks the environment where to find Python. The first hashbang would fail if you moved your program to a machine that did not store Python in that particular location. 

The reason we use `env` in this way is to decouple our script from the software that supports it. If we later need to change Python interpreters, or the compiler that created them, we can simply change the `$PATH` variable, point to a different version of the software, and use that one instead.

## Exporting to the subshell

In the example above, we set a variable directly in the shell. However, if we ran the `env` command, our variable would not be present. The current shell is therefore *distinct* from the environment. If we made a new shell by running the `bash` command, we would not have access to the variables in the parent shell.

This programming concept is called [scope](https://en.wikipedia.org/wiki/Scope_(computer_science)) and varies between languages.

To force a variable to exist in any new shells, which we call *subshells*, you must use the `export` command. You can export a variable when you define it, or export it after it has been defined. 

~~~
$ INPUT_FILE=~/input_file.txt
$ echo $INPUT_FILE
/home-4/rbradle8@jhu.edu/input_file.txt
$ export INPUT_FILE
$ export INPUT_FILE=~/input_file_2.txt
$ echo $INPUT_FILE
/home-4/rbradle8@jhu.edu/input_file_2.txt
~~~

After you export a variable, then that variable will exist in any new subshell. In a moment we will confirm that the variable exists in the environment, but you are welcome to inspect the `env` yourself if you wish.

{% include note.html content="Subshels are important because SLURM jobs run in a subshell of your login shell. This means that they ***inherit*** your current environment and therefore have access to all of the same variables." %}

### Subshell inception

You can confirm that a new bash shell does not include the new variable. *Please be mindful of whether you are in a subshell or not.* The following example must be executed *exactly* as written or you might accidentally drop your connection. This exercise demonstrates environment inheritance. We use the `$$` to get the process ID (PID) of the current shell so we know which shell we occupy.

~~~
$ DATA_SOURCE=~/input_file.txt
$ echo $DATA_SOURCE
/home-4/rbradle8@jhu.edu/input_file.txt
$ echo $$
126890
$ bash
$ echo $$
127246
$ echo $DATA_SOURCE

$ exit
$ echo 
126890
$ export DATA_SOURCE
$ bash
$ echo $$
127542
$ echo $DATA_SOURCE
/home-4/rbradle8@jhu.edu/input_file.txt
~~~

Note the blank line above shows that the variable has not been exported. Once we export it, the subshells can access it.

## Pipes and regular expressions

The pipe operator `|` allows you to send the output of one command to the inputs of another. We will use two examples to illustrate this technique, however the underlying code is [extremely flexible](https://stackoverflow.com/questions/9834086/what-is-a-simple-explanation-for-how-pipes-work-in-bash).

### Scrolling through the queue

When you run the `squeue -p parallel` command to check the `parallel` partition in SLURM, the output is extremely verbose. To scroll through the output slowly, you can pipe the result to the `more` command. Recall the we previously used `more somefile` to view a single file. The `more` command can also receive a pipe.

~~~
$ squeue -p parallel | more
~~~

Use the <kbd>spacebar</kbd> to advance or <kbd>q</kbd> to exit early.

### Searching text

The output of `env` is extremely long. To search a stream of text for a single phrase, we can pipe the results to the *generalized regular expression* or `grep` command to search for a phrase. First, export a variable called `$DATA_SOURCE`, then you can search for either its name or value with grep as follows.

~~~
$ export SPECIAL_VALUE=~/some_input.txt
$ env | grep SPECIAL
SPECIAL_VALUE=/home-4/rbradle8@jhu.edu/some_input.txt
$ env | grep some_input
SPECIAL_VALUE=/home-4/rbradle8@jhu.edu/some_input.txt
$ SPECIAL_VALUE=8675309
$ env | grep 5309
SPECIAL_VALUE=8675309
~~~

Regular expressions are an [invaluable method for parsing text](https://www.regular-expressions.info/) in many programs.

<a class="btn btn-primary" href="shortcourse_bash.html">Next: software modules</a>

