---
layout: post
title: Calculate length of all sequences in an multi fasta file
excerpt: "A simple solution to get fasta length using python"
date: 2014-04-14T15:21:00+00:00
modified: 2014-04-14T15:21:00+00:00
tags: [Arun Seetharam, fasta, length, stats, bioinformatics, bash]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

Sometimes it is essential to know the length distribution of your sequences. It may be your newly assembled scaffolds or it might be a genome, that you wish to know the size of chromosomes, or it could just be any multi fasta sequence file. A simple way to do it is using `biopython`.

For example save this script as `seq_length.py`

```python
#!/usr/bin/python
from Bio import SeqIO
import sys
cmdargs = str(sys.argv)
for seq_record in SeqIO.parse(str(sys.argv[1]), "fasta"):
 output_line = '%s\t%i' % \
(seq_record.id, len(seq_record))
 print(output_line)
```

To run,

```bash
chmod +x seq_length.py
seq_length.py inpput_file.fasta
```

This will print length for all the sequences in that file.
