---
layout: post
title: Split multi fasta sequence file
excerpt: "If you need to break a large fasta file with multiple sequences into individual file, use this trick!"
date:  2013-04-04T15:21:00+00:00
modified:  2013-04-04T15:21:00+00:00
tags: [Arun Seetharam, awk, split, fasta, bioinformatics, bash]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

Sometimes it is necessary to split a large file containing several sequences (fasta format) in to individual files. I do this by a simple 'awk' command where i separate sequences based on regular expression match and then write it to a file numbered sequentially. It is easy and quick!

```bash
awk '/^>/{s=++d".fasta"} {print > s}' <inputFile>
```
