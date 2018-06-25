---
title: Linux su root和sudo su
tags: Linux
abbrlink: 6b4d1db7
date: 2018-05-05 23:43:02
---



来自https://askubuntu.com/questions/86095/does-executing-sudo-su-and-su-root-do-the-same-thing?utm_medium=organic&utm_source=google_rich_qa&utm_campaign=google_rich_qa


Does executing sudo su and su root do the same thing?What are the technical differences between the two? The only thing I noticed is that

`sudo su`
requires you to enter your own password (assuming you're not root)

`su root`
requires you to enter root's password. However both seem to log you into the root user account.


The second command cannot be executed in a default Ubuntu installation, where the root account is not enabled.

But supposing you have unlocked the root account giving him a password, the two commands could only differ in the environment and shell variable set, I think. Compare the output of env in the two situations, and maybe also the output of set to see the differences.