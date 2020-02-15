---
title: Required software
permalink: shortcourse_prereq.html
sidebar: shortcourse_sidebar
folder: shortcourse
toc: false
---

Blue Crab is a remote computer which requires a secure connection and two-factor authentication (2FA). To accomplish this, we will use [secure shell](https://en.wikipedia.org/wiki/Secure_Shell) to dial in. First, we will briefly cover installation instructions for the following machines.

1. [Windows 10](#win10)
2. [Windows 7](#win7)
3. [MacOS](#macos)

If you have signed up for an account and already have access to `ssh`, you can log on using the following information. Note that the usernames on MARCC include the entire institution handle.

~~~
hostname: login.marcc.jhu.edu
username: name@institution.edu
~~~

## Windows 10 {#win10}

Windows 10 includes `ssh` [by default](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_overview), hence no installation is necessary. To use `ssh`, open the "command prompt" from the search box in the task bar and type `ssh` to get started.

![The command prompt in Windows.](figs/snap-win-prompt.png)

Both `ssh` and `scp` are available for logging on to MARCC and transferring files.

## MacOS {#macos}

Similarly, MacOS ships with a terminal that requires no additional installation. Open a spotlight search with <kbd>command</kbd> + <kbd>spacebar</kbd> and type `terminal` to open a terminal.

![The MacOS terminal.](figs/snap-mac-terminal.png)

Both `ssh` and `scp` are available for logging on to MARCC and transferring files.

## Windows 7 {#win7}

To use `ssh` in Windows 7 you must download and install a program called [PuTTY](https://www.putty.org/). After you start PuTTY, you must enter the hostname in the "Session" configuration. It is best to save your session for later.

![putty window.](figs/snap-putty-main.png)

You should also visit the `Connection, Data` section of the configuration and enter your username.

![putty window.](figs/snap-putty-user.png)

## Tunneling windows

Users who wish to use graphical user interfaces (GUIs) other than our portal tools (discussed later) should also install an `X11` client. You can use [Xquartz](https://www.xquartz.org/) for MacOs and [Xming](https://sourceforge.net/projects/xming/) for Windows if necessary.

<a class="btn btn-primary" href="shortcourse_login.html">Next: logging on</a>