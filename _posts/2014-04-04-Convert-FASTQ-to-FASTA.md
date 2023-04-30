---
layout: post
title: Convert FASTQ to FASTA
excerpt: "Many different ways for a simple conversion of fastq format to fasta format"
date: 2014-04-04T15:21:00+00:00
modified: 2014-04-04T15:21:00+00:00
tags: [Arun Seetharam, fastq, fasta, format, bioinformatics, bash]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

### Using SED

```bash
sed -n '1~4s/^@/>/p;2~4p' INFILE.fastq > OUTFILE.fasta
```

NOTE: This is, by far, fastest way to convert FASTQ to FASTA

### Using PASTE

```bash
cat INFILE.fastq | paste - - - - | \\
cut -f 1, 2| sed 's/@/>/'g | \\
tr -s "/t" "/n" > OUTFILE.fasta
```

### EMBOSS:seqret

```bash
seqret -sequence reads.fastq -outseq reads.fasta
```

### Using AWK

```bash
cat infile.fq | \\
awk '{if(NR%4==1) {printf(">%s\n",substr($0,2));} else if(NR%4==2) print;}' > file.fa
```

### FASTX-toolkit

```
fastq_to_fasta -h
usage: fastq_to_fasta [-h] [-r] [-n] [-v] [-z] [-i INFILE] [-o OUTFILE]
# Remember to use -Q33 for illumina reads!
version 0.0.6
       [-h]         = This helpful help screen.
       [-r]         = Rename sequence identifiers to numbers.
       [-n]         = keep sequences with unknown (N) nucleotides.
                    Default is to discard such sequences.
       [-v]         = Verbose - report number of sequences.
                    If [-o] is specified, report will be printed to STDOUT.
                    If [-o] is not specified (and output goes to STDOUT),
                    report will be printed to STDERR.
       [-z]         = Compress output with GZIP.
       [-i INFILE]  = FASTA/Q input file. default is STDIN.
       [-o OUTFILE] = FASTA output file. default is STDOUT.
```

### Using BioAWK

```bash
bioawk -c fastx '{print ">"$name"\n"$seq}' reads.fastq | fold > reads.fasta
```
