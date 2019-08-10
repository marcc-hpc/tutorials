---
title: Editing text files
permalink: shortcourse_text.html
sidebar: shortcourse_sidebar
folder: shortcourse
summary: Learn how to edit a text file so you can start writing scripts and code on Blue Crab.
toc: false
---

There are three ways we interact with *Blue Crab*. We issue commands from the terminal, inside the BASH shell; we transfer files to and from the machine; and, we edit text files. Unless you prepare an entire calculation on your local machine and then upload ot to *Blue Crab*, you will almost certainly need to edit a text file.

> This section covers the bare minimum required to use `vi` to edit a text file. If you have another preferred editor, or are otherwise comfortable editing files in Linux, you are free to skip to the next section.

## Making a blank file

Before we edit a file, we will first create a blank file using the `touch` command.

~~~
$ touch my_script
$ ls -l
~~~

The file listing should show that your file has been created. Let's see what's inside. Use the `more` and `cat` commands to show the contents

~~~
$ more my_script
$ cat my_script
~~~

Both of these report that the file is empty. The `more` command is useful for scrolling through long text files while using the <kbd>spacebar</kbd> to advance your view. The `cat` command is short for "concatenate" and takes multiple arguments, printing the contents of each file argument to the screen. This can be useful for joining many text files into one larger text file.

## Editing the file

Open the file using `vi` or `vim`, which is short for "visual".

~~~
$ vi my_script
~~~

{% include note.html content="`Vim` is a **two-mode** editor which means that you must carefully pay attention to which mode you are using. It has innumerable advantages over *modeless* editors which are said to make your fingers gnarled from pressing the <kbd>control</kbd> key so much." %} 

The visual editor looks like this:

![a new file in vim](figs/snap-vim-blank.png)

Sometimes there is useful information in the bottom row. The bottom right shows the line and character number we have selected. The tilde characters (`~`) indicate the absence of text.

We are currently in the command mode. Use the <kbd>i</kbd> key to switch to the insert mode and the <kbd>escape</kbd> key to return to the command mode. ***It is absolutely critical that you understand the difference between these modes.*** You can always tell when you are in the insert mode because the word `-- Insert --` appears in the bottom of the screen.

In the insert mode, you can enter text. Let's write the `echo` command shown below.

![a new file in vim](figs/snap-vim-insert.png)

A batch script is a text file that holds a series of commands (a "batch") that you might enter directly into the terminal or shell (N.b. these words mean the same thing). In the example above we have written a command that echoes some text to the terminal. The `echo` command is actually a program which takes many arguments, all text, and simply repeats them.

Now that we have entered some text, we will 


<a class="btn btn-primary" href="shortcourse_transfer.html">Next: editing text</a>