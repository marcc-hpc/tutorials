---
title: Parallel job arrays
permalink: shortcourse_jobarrays.html
sidebar: shortcourse_sidebar
folder: shortcourse
toc: false
---

The scheduler provides the simplest method for running parallel computations. SLURM schedules thousands of simultaneous calculations on *Blue Crab* and will gladly execute many of your jobs at once, as long as there are available resources.

This means, that in contrast to the language-specific parallelism methods required by OpenMP, MPI, and various threading methods built into languages like Python, Matlab, and R, SLURM can provide [embarassingly parallel](https://en.wikipedia.org/wiki/Embarrassingly_parallel) calculations. These calculations, more generously called "perfectly parallel" do not require any exchange of information between individual jobs which would otherwise require a high-speed network and intelligent algorithm for communicating these data. They scale *perfectly* which means that twice as much calculation can be completed in the same amount of time with twice as much hardware. This is not always true for *true* or imperfect parallel calculations.

## SLURM Job Arrays

A SLURM job array is a method for marking a single batch script to run many times. Imagine a calculation which requires 100 iterations, each with a different input file, labelled `input_1.txt` through `input_100.txt`. To run this calculation in an example program named `compute_something` in the current directory, you could run one of these iterations by submitting the following script with `sbatch script.sh`.

~~~
#!/bin/bash
#SBATCH -p shared
#SBATCH -t 2:0:0
#SBATCH -c 1
./compute_something input_1.txt
~~~

Instead of copying this script 100 times for each of the input files, you can use a job array to submit 100 separate jobs *with one script*.

~~~
#!/bin/bash
#SBATCH -p shared
#SBATCH -t 2:0:0
#SBATCH -c 1
#SBATCH --array=1-100
./compute_something input_$SLURM_ARRAY_TASK_ID.txt
~~~

Submit all 100 jobs with `sbatch script.sh`. Each job will have a job number with an underscore, indicating the "task" ID within the job, for example `12345_1` for the first task, and so on.

## Customizing job arrays

If you wish to throttle your jobs because they use the same set of files on our storage system, you can include a percent sign to run a limited number of concurrent jobs, for example, with `--array=1-100{% raw %}%{% endraw %}4` to run only four at one time. You can also request custom task IDs with `--array=1,3,5,7` and a custom step size with `--array=1-7:2`.

In your job scripts, several BASH variables may help you control the inputs and ouputs of your calculation. The `$SLURM_JOBID` is sequential, and includes an underscore to denote both the initial submitted job and the task. The `$SLURM_ARRAY_JOB_ID` is the same for all jobs in the array while the `$SLURM_ARRAY_TASK_ID` is equal to the index supplied by the array option.

## When to use Job Arrays

You should use job arrays whenever it easy to customize a parameter sweep with a single index. It is also essential that each job last longer than a few minutes, otherwise the inherent lag time required for a highly-decentralized system like SLURM to schedule your job will make it prohibitively expensive. Other parallel methods require far less overhead and may be more suitable for your calculations.

{% include note.html content="Job arrays represent one of many ways to parallelize your code to take advantage of the massive scale of *Blue Crab*. There are many other ways to parallelize your code." %}

<a class="btn btn-primary" href="shortcourse_bash.html">Next: shell environments</a>
