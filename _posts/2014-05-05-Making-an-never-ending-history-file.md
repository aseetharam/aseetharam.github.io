---
layout: post
title: Making an never ending history file
excerpt: "Problem: your history file in bash is not big enough to hold every single command you type"
date: 2014-05-05T15:21:00+00:00
modified: 2014-05-05T15:21:00+00:00
tags: [Arun Seetharam, bashrc, history, bioinformatics, bash]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

One useful feature in bash is that you can recall previously used commands (using arrow keys). You can also recursively search your history by pressing `Ctrl + r` and typing the command name, which brings up the matching commands. It will also lets you cycle through all matching commands by pressing `Ctrl + r` repeatedly. This is only helpful, if your history file is big. By default, `$HISTFILE` holds only limited number of entries (1000 lines or commands). You can easily hack it, so that you can store unlimited number of entries. Simply follow these steps:
First, in your `.bashrc` file, set these variables

```bash
export HISTFILESIZE=
export HISTSIZE=
export HISTFILE=~/.bash_eternal_history
```

This will make your history file unlimited! Other useful feature that you can use is setting `$HISTTIMEFORMAT` variable. This will write time stamps in the history file, marked with the history comment character, so they may be preserved across shell sessions.

```bash
export HISTTIMEFORMAT="[%F %T] "
```

Now start making a never ending history!
