---
layout: post
title: Running interactive jobs on clusters
excerpt: "quick introduction for using screen, a program that keeps your terminal alive even after you disconnect/close your terminal"
date: 2014-04-08T15:21:00+00:00
modified: 2014-04-08T15:21:00+00:00
tags: [Arun Seetharam, screen, interactive, cloud, bioinformatics, bash]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

Sometimes you might have to run the jobs on clusters interactively (programs that aren't optimized to run by itself, programs that need user input for various steps or simply for de-bugging purposes). In those cases, it is better to run them in virtual terminals using `screen` command. I normally use this to order contigs using  Mauve, where I need large memory/processors. Running this on head node, will get me an email from sysadmin.  Here is an example:

After you SSH into the head node, open up a new `screen` session, assume that you will be running `program1` here.

```bash
screen -S program1
```

This will begin the new terminal, that is virtual and doesn't require a running computer to keep it alive (and thus it can run even after you close all your windows, turn off your PC). You can detach from this `screen` any time by pressing `Ctrl+a` followed by `d`. This will bring you back to your first terminal which you SSH'ed before.

You can open several of these screen sessions, for `program2`, `program3` etc.,

```bash
screen -S program2
#press 'Ctrl+a' followed by 'd'
[detached]
screen -S program3
#press 'Ctrl+a' followed by 'd'
[detached]
```

Once you are back on head node, you can see all the running screen sessions by typing:

```bash
screen -ls
There are screens on:
        1234.program1   (Detached)
        1235.program2   (Detached)
        1236.program3   (Detached)
```

You can re-attach to any `screen` you want, by:

```bash
screen -r 1234.program1
```

To close a particular `screen` session, simply attach to that session and enter `Ctrl+a` followed by `:` and type `quit`.
To get help, enter `Ctrl+a` followed by `?`

Once you open a screen session, you can request a interactive run access via PBS script using:

```bash
qsub -I -l mem=256Gb,nodes=1:ppn=32,walltime=48:00:00 -N program1
# this might depend on your cluster settings
```

After your job gets accepted, you can start running any program just like you would on the head node, without being killed by sysadmin.

Alternatively, you can also add this as alias to your .bashrc file, so that you can quickly open an interactive session, just by typing `qlive`

```bash
alias qlive="qsub -I -l mem=256Gb,nodes=1:ppn=32,walltime=48:00:00 -N stdin"
```

I hope this helps!
