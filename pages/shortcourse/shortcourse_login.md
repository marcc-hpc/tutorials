---
title: Login to the home directory
permalink: shortcourse_login.html
sidebar: shortcourse_sidebar
folder: shortcourse
toc: false
---

## How to connect

Given a username `me@school.edu` and the MARCC hostname `login.marcc.jhu.edu`, you can connect with the following command:

~~~ bash
ssh me@school.edu@login.marcc.jhu.edu
~~~

You will be prompted for your two-factor authentication (2FA) token.

{% include note.html content="If you incorrectly enter your 2FA token or your password you must *wait for the next one* or else you will be automatically rejected. Wait for the next token before trying again. If your clock is incorrect, your codes will always fail." %}


![logging in for the first time](figs/snap-connect-1.png)

The prompt `[rbradle8@jhu.edu@bc-login02 ~]$` includes your username and login node. We have three login nodes. The dollar sign represents a typical linux command prompt. We issue commands from the prompt. When you log in we tell your your storage usage.

## Resetting your password or 2FA

Our website offers links for [resetting your password](https://password.marcc.jhu.edu/?action=sendtoken) and [retrieving your 2FA tokens](https://password.marcc.jhu.edu/?action=qrretrieve). If these methods fail, please email our help desk at [marcc-help@marcc.jhu.edu](mailto:marcc-help).

## Where am I?

Use the `ls -l` command to *list* the contents of your home directory.

![listing the contents of our home directory](figs/snap-connect-2.png)

The listing above provides several columns of (somewhat) useful information. 

1. A permission code e.g. `drwxr-xr-x`
2. Number of hard links (not relevant yet)
3. Owner of the file or folder
4. The group
5. The size in bytes
6. The time stamp of the last modification
7. The name of the object, with a link location if applicable

Use the `realpath .` command to see the full path to your home directory. Note that `.` means "here" i.e. the current folder, `..` means the parent folder, and `~` always refers to your home directory.

~~~ bash
$ realpath .
/home-net/home-4/rbradle8@jhu.edu
~~~

In the code block above you can tell that I have executed a command because it is prefixed with `$` (we omit the full prompt for brevity). We will use this convention throughout the tutorial.

The difference between the "real" path above and the name `~` is purely symbolic. The storage system allows symbolic links, including the ones shown with `ls -l` pictured above.

### Storage locations

You can see from the `ls -l` command that we have several symbolic links in our home directory, which itself a symbolic link. These links point to different file systems.

1. `~/scratch` is your personal scratch space. Use it to store I/O-intensive data used by your calculations. This Lustre filesystem has a limit of 6 months.
2. `~/work` is your group scratch space. Use it for data shared between group membners. This also occupies the Lustre filesystem and has a limit of 6 months.
3. `~/data` is your group data location on our ZFS filesystem. This filesystem is larger and slower than the high-performance Lustre filesystem. Although it has no time limit, you should plan to migrate your data before we retire the hardware at then end of its lifespan (typically 5-7 years).
4. `~` is the home directory and is designed for compiling and executing programs. You may also compile and execute programs from `~/data`. We recommend using Lustre to compile code.

{% include note.html content="All storage is not created equal. Please think carefully about where your data is located. The scratch system is best for fast I/O operations during your calculations and should be regularly archived to the `~` and `~/data` folders, which should eventually be migrated off of the machine at the conclusion of your research projects. We recommend that you develop a complete plan for the entire lifecycle of your data." %}

{% include note.html content="Scratch is **not backed up** and only the base allocations on `~/data` are backed up without an additional fee. All users should *always* back up their data offsite." %}

## Storage quotas

If you put too much data in your home directory `~` you will be forced to remove files with an alternate method called Globus discussed later. You can track your storage usage with two commands. The `zfs_home_quota` command tells you the amount of data in your home directory.

~~~
$ zfs_home_quota 
+--------------------------------------------+
|  $HOME quota: login is disabled when full  |
+-------------+-------------+----------------+
|    Usage GB |      Max GB |   Last Updated |
+-------------+-------------+----------------+
|       5.178 |      20.000 |        05:02AM |
+-------------+-------------+----------------+
~~~

The `scratchquota` command shows the Lustre filesystem usage on `~/work` and `~/scratch` for your group.

~~~
$ scratchquota
+-------------------------------------------------------------------------------------+
|  Scratch quota: group g16. Last Update: 05:31AM. Use 'scratchquota -f' to see more  |
+---------------------+---------------+---------------+---------------+---------------+
|      Group/Username |      Usage TB |        Max TB |         Files |     Max Files |
+---------------------+---------------+---------------+---------------+---------------+
|                 g16 |         0.000 |         0.000 |             0 |             0 |
|            rbradley |         0.005 |             - |         42142 |             - |
+---------------------+---------------+---------------+---------------+---------------+
~~~

You may be included in a group with no scratch quota because it serves another purpose, in this case membership in the group which uses [Gaussian 16](https://gaussian.com/gaussian16/), a licensed software.

### Limits

Each user is entitled to only `20GB` in their home directory. Research groups are typically allocated `1TB` on Lustre, shared between each users' `~/scratch` and the group's `~/work` directory, as well as `1TB` on the shared `~/data` folder. 

## The BASH Shell

As with any Linux system, we interact with the machine through a "shell" which receives commands in a text format. We use the Bourne Again SHell (BASH) by default. Learning about the shell is beyond the scope of this course, but there are ample online resources. For now, consider the following rules.

1. Capitalization matters. By convention we avoid capital letters and spaces.
2. Each command is executed with the <kbd>enter</kbd> key. 
3. Each command is composed of arguments separated by a space. 
4. Order typically matters. The shell uses extremely strict syntax.

The [Bash for beginners tutorial](https://www.tldp.org/LDP/Bash-Beginners-Guide/html/) is a good place to learn more.

<a class="btn btn-primary" href="shortcourse_text.html">Next: editing text</a>
