---
layout: post
title: Calculating moving average in R
excerpt: "attach moving average field for your file"
date: 2014-07-07T15:21:00+00:00
modified: 2014-07-07T15:21:00+00:00
tags: [Arun Seetharam, R, moving, average, bioinformatics, bash]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

When the raw data obtained from an experiment is too noisy and you need to smooth-en it to better represent the trend, you need to calculate the `moving average` . Moving average is nothing but average of `n` previous numbers, with a specific step size.  Let me give an example:  if there are 100 numbers, then moving average is calculated by averaging 1-15, 2-16, 3-17 and so on.  Here, the n is 15 and step size is 1.




The R script to do this:

```bash
datain <- read.table("input.txt", header=1)
field2 = datain[,2]
coef15 = 1/15
mvavg15 = filter(field2, rep(coef15, 15), sides=1)
jpeg('input.jpg')
plot(mvavg15, type="l", main="Plot Title", xlab="X label", ylab="Y label")
dev.off()
```

Here, the data is assumed to be 2 column, first with serial number and second with value.
