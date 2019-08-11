---
title: "Interactive jobs"
permalink: shortcourse_interact.html
sidebar: shortcourse_sidebar
folder: shortcourse
toc: false
---

You may wish to perform a "live" calculation on a large amount of compute hardware. Use the `interact` command to directly access the compute nodes. This command puts you *inside* of a SLURM job.

## The high-availability `express` partition.

All users have access to 6 cores for up to 12 hours with almost zero wait time if they use our `express` partition. It is best to avoid using the login nodes for compute-intensive tasks because these are shared resources. The login nodes automatically terminate jobs which use too much memory, and our staff also  terminate costly calculations on these nodes. 

The `express` partition is ideal for post-processing and analysis and offers processors equivalent to a workstation with the added benefit of full access to all of your storage. You may also wish to use the high-availability `debug` partition for compiling codes.

You are free to use `interact` and the SLURM `srun` command to use any partition on *Blue Crab*, however there is a non-trivial wait time associated with the most in-demand partitions. If you are using MPI, you should consult the [associated SLURM documentation here](https://www.open-mpi.org/faq/?category=slurm).

## Using the `interact` command

Most users can access the compute nodes directly with a command like this one:

~~~
interact -p debug -c 4 -t 2:0:0
~~~

This command requests 4 cores on the `debug` partition for two hours. This partition is ideal for debugging or compiling and has a lot wait time because it has a two-hour time limit. If you omit the paritition, you will be assigned to the `express` partition, which is the default.

The command will report that it has started a SLURM job, at which point it "drops you into a BASH shell" on the desired compute node. Once inside the job, you can check common BASH variables to see which resources you have access to. In the following example I have requested 1 GPU (with the `-g` flag) and its 6 associated CPUs.

~~~
[rbradle8@jhu.edu@bc-login03 ~]$ interact -p gpuk80 -g 1 -c 6 -t 0:30:0
Tasks:    1
Cores/task: 6
Total cores: 6
Walltime: 0:30:0
Reservation:   
Queue:    gpuk80
Command submitted: salloc -J interact -N 1-1 -n 1 -c 6 --time=0:30:0 --gres=gpu:1 -p gpuk80 srun --pty bash
salloc: Pending job allocation 35766078
salloc: job 35766078 queued and waiting for resources
salloc: job 35766078 has been allocated resources
salloc: Granted job allocation 35766078
+----------------------------------------------------------------+
|   Group Balance. Use 'sbalance' or 'sbalance -f' to see more   |
+-----------+---------------+---------------+---------+----------+
|   Account | User/NumUsers | Allocation(h) | Used(h) | Usage(%) |
+-----------+---------------+---------------+---------+----------+
| jcombar1* |            18 |        550000 |   54666 |     9.94 |
+-----------+---------------+---------------+---------+----------+
+--------------------------------------------+
|  $HOME quota: login is disabled when full  |
+-------------+-------------+----------------+
|    Usage GB |      Max GB |   Last Updated |
+-------------+-------------+----------------+
|       5.178 |      20.000 |        04:03PM |
+-------------+-------------+----------------+
[rbradle8@jhu.edu@gpu010 ~]$ hostname
gpu010
[rbradle8@jhu.edu@gpu010 ~]$ nproc
6
[rbradle8@jhu.edu@gpu010 ~]$ echo $CUDA_VISIBLE_DEVICES 
0
[rbradle8@jhu.edu@gpu010 ~]$ echo "Hello, $(hostname)."
Hello, gpu010.
[rbradle8@jhu.edu@gpu010 ~]$ exit
exit
salloc: Relinquishing job allocation 35766078
[rbradle8@jhu.edu@bc-login03 ~]$
~~~

Note that the interact command calls a somewhat more complicated `salloc` command, which is equivalent to `sbatch` but inserts your terminal into the job itself. Every time you invoke a login shell, you see your quota. Note that the `$CUDA_VISIBLE_DEVICES` is a BASH variable that indicates that you are using the first (`0`) device. This variable is completely blank if there are no GPUs.

It is important to note that your prompt has changed, indicating that you are occupying a compute node. When your calculation is complete, use `exit` to leave, otherwise you will be ejected once time expires.

<a class="btn btn-primary" href="shortcourse_jobarrays.html">Next: parallel job arrays</a>
