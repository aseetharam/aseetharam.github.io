---
layout: post
title: Extract sequences from a FASTA file given the co ordinates
excerpt: "having a sequence co-ordinates and you need the sequence? use this trick!"
date: 2014-04-07T15:21:00+00:00
modified: 2014-04-07T15:21:00+00:00
tags: [Arun Seetharam, fasta, bed, coords, bioinformatics, bash]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

Many times, it might be essential to grab  a portion of a FASTA sequence to perform downstream analyses. One such case would be to extract all the genes given the whole genome (like all chloroplast and mitochondrial genes using just the GFF file). There are are several ways to do this.

1. If you are familiar with Galaxy, there is a "Extract Genomic DNA" tool that can do this job. You can use either a GFF or GTF datasets (if not in this format you can easily convert it). This is pretty straight forward.







2. Using BEDTools, a utility called 'fastaFromBed' can do this. Simply,

```bash
fastaFromBed -fi in.fasta -bed regions.bed -fo out.regions.fa
```

3. GLIMMER package as 2 scripts that can be used to extract sequences based on co-ordinates.

```bash
extract [options] sequence coords
```

This program reads a genome sequence and a list of coordinates for it and outputs a multifasta file of the regions specified by the coordinates

```bash
multi-extract [options] sequences coords
```

This program is a multi-fasta version of the preceding program
