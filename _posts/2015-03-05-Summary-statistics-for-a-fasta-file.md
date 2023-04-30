---
layout: post
title: Summary statistics for a fasta file
excerpt: "Get detailed summary for your fasta file"
date: 2015-03-05T15:21:00+00:00
modified: 2015-03-05T15:21:00+00:00
tags: [Arun Seetharam, summary, statistics, fasta, bioinformatics, bash]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

On my previous post (Calculate length of all sequences in an multi-fasta file) , one of the user commented that it would be useful to have summary statistics for the sequence length distribution or histogram. So, this simple script can be used along with the previous script to get the summary statistics.

```bash
#!/bin/sh
# reads stdin and prints summary statistics
# total, count, mean, median, min and max
# you can pipe this through scripts or redirect input <
# modified from http://unix.stackexchange.com/a/13779/27194
sort -n | awk '
  $1 ~ /^[0-9]*(\.[0-9]*)?$/ {
    a[c++] = $1;
    sum += $1;
  }
  END {
    ave = sum / c;
    if( (c % 2) == 1 ) {
      median = a[ int(c/2) ];
    } else {
      median = ( a[c/2] + a[c/2-1] ) / 2;
    }
    OFS="\t";
    { printf ("Total:\t""%'"'"'d\n", sum)}
    { printf ("Count:\t""%'"'"'d\n", c)}
    { printf ("Mean:\t""%'"'"'d\n", ave)}
    { printf ("Median:\t""%'"'"'d\n", median)}
    { printf ("Min:\t""%'"'"'d\n", a[0])}
    { printf ("Max:\t""%'"'"'d\n", a[c-1])}
  }
'
```

For using this script, first change the permissions and pipe it through the output of the previous script (seq_length.py, see post: Calculate length of all sequences in an multi-fasta file).

```bash
chmod +x summary_stats.sh
seq_length.py input_file.fasta | cut -f 2 |summary_stats.sh
```

You will see formatted output as follows

```
Total:  1,825,408
Count:  1,155
Mean:   1,580
Median: 1,360
Min:    1,001
Max:    12,972
```

For getting a histogram, I suggest using a per-existing package called data_hacks (on github). The installation is fairly simple (see README), can be used with the above scripts. Once you download/installed the package, run the above script to get the min/max values. This can be plugged in to the data_hacks script to get the histogram (you can also save the data to be later plotted using any other ways, too).

```bash
seq_length.py input_file.fasta |cut -f 2 | histogram.py --percentage --max=12972 --min=1001
```

The output you will get is:

```
# NumSamples = 1155; Min = 1001.00; Max = 12972.00
# Mean = 1580.439827; Variance = 624229.340751; SD = 790.081857; Median 1360.000000
# each ∎ represents a count of 13
 1001.0000 -  2198.1000 [  1019]: ∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎∎ (88.23%)
 2198.1000 -  3395.2000 [   108]: ∎∎∎∎∎∎∎∎ (9.35%)
 3395.2000 -  4592.3000 [    17]: ∎ (1.47%)
 4592.3000 -  5789.4000 [     6]:  (0.52%)
 5789.4000 -  6986.5000 [     2]:  (0.17%)
 6986.5000 -  8183.6000 [     0]:  (0.00%)
 8183.6000 -  9380.7000 [     1]:  (0.09%)
 9380.7000 - 10577.8000 [     1]:  (0.09%)
10577.8000 - 11774.9000 [     0]:  (0.00%)
11774.9000 - 12972.0000 [     1]:  (0.09%)
```

I hope this post helps!
