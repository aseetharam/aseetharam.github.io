---
layout: post
title: Bash loops for productivity
excerpt: "some commonly use bash loops that can boost your productivity!"
date: 2013-04-04T15:21:00+00:00
modified: 2013-04-04T15:21:00+00:00
tags: [Arun Seetharam, bash, bioinformatics, loop, if, while]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

Working in bash is so much fun! If you spend enough time in terminal, then you might get addicted to it and never like the gui windows. There are commands (especially loops) that can save you lot of time. They are very useful to do some routine stuff. My favorite loops are as follows:
Loops through all the files with txt extension and performs the action

```bash
for f in *.txt; do yourcommand $f >$f.out; done
```

Read file line by line and run command on it

```bash
while read line; do yourcommand $line; done<FileToRead.txt
```

Other variation of this above command (extremely useful when you have to read arguments from a file:

```bash
while read fld1 fld2 fld3; do YourCommand -a $fld1 -b $fld2 -c $fld3 > $fld1.$fld2.$fld3.txt; done<FileToRead.txt
```

A simple loop for a set of numbers (you can also use {a..z} etc., or mix them)

```bash
for i in {1..10}; do echo $i; done
```

Another variation of the above command

```bash
for i in {1..22} X Y; do echo "human chromosome $i"; done
```

I hope these will help you too!
