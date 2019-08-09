---
title: Prerequisites
permalink: shortcourse_connect_prereq.html
sidebar: shortcourse_sidebar
folder: shortcourse
toc: false
tags: [connect]
---

Blue Crab is a remote computer which requires a secure connection and two-factor authentication (2FA). To accomplish this, we will use [secure shell](https://en.wikipedia.org/wiki/Secure_Shell) to dial in. First, we will briefly cover installation instructions for the following machines.

1. [Windows 10](#win10)
2. [Windows 7](#win7)
3. [MacOS](#macos)

## Windows 10 {#win10}

Windows 10 includes `ssh` [by default](https://docs.microsoft.com/en-us/windows-server/administration/openssh/openssh_overview), hence no installation is necessary. To use `ssh`, open the "command prompt" from the search box in the task bar and type `ssh` to get started.

![The command prompt in Windows.]({{page.root}}/figs/snap-win-prompt.png){:.normimg}

Both `ssh` and `scp` are available for logging on to MARCC and transferring files.

## MacOS {#macos}

Similarly, MacOS ships with a terminal that requires no additional installation. Open a spotlight search with <kbd>command</kbd> + <kbd>spacebar</kbd> and type `terminal` to open a terminal.

![The MacOS terminal.]({{page.root}}/figs/snap-mac-terminal.png){:.normimg}

Both `ssh` and `scp` are available for logging on to MARCC and transferring files.

## Windows 7 {#win7}

To use `ssh` in Windows 7 you must download and install a program called [`PuTTY`](https://www.putty.org/).

## Other terminals

FIXME

