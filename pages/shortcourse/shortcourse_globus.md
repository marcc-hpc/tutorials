---
title: Using Globus to transfer files
permalink: shortcourse_globus.html
sidebar: shortcourse_sidebar
folder: shortcourse
toc: false
---

A program called *Globus* offers a robust, high-speed transfer method which can continue a transfer even if it is interrupted. Visit [globus.org](https://www.globus.org) and click "log in" to visit the login portal.

Globus offers a federated login with your institution's credentials. If your institution is not on the list, you can sign up for a free account.

![the Globus login portal](figs/snap-globus-auth.png)

This portal will take you to your institution-specific login page. Once you enter your credentials, you will return to globus.

Click the "endpoints" button and then search for "MARCC" in the search bar. The top result should be the *Managed Public Endpoint* for MARCC. Click that result, and click "activate". You will have to authenticate using your password. Once you authenticate, you will have accesss to a file manager.

![the Globus login portal](figs/snap-globus-files.png)

Globus allows you to initiate transfers of large files from your local machine (sometimes called a personal endpoint) to so-called managed endpoints, one of which resides on MARCC. Globus transfers are asynchronous, which means that they can happen in pieces, and will continue after an interruption in connectivity. For this reason, they are somewhat more reliable than standard file transfers.

<a class="btn btn-primary" href="shortcourse_slurm.html">Next: the scheduler</a>
