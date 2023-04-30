---
layout: post
title: Calculating genome size from flow cytometry results
excerpt: "Flow cytomerty results in picogram, but you need it in Mb?"
date: 2014-04-10T15:21:00+00:00
modified: 2014-04-10T15:21:00+00:00
tags: [Arun Seetharam, genomesize, flowcytometry, bioinformatics, bash]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

Flow cytometry (FCM) is a laser-based, biophysical technology employed in various molecular biology studies. Many applications of FCM include cell counting, cell sorting, biomarker detection and protein engineering. It is also now being used to measure nuclear DNA content in plants (using DNA-selective fluorochromes). It is replacing older methods of estimating DNA such as Feulgen densitometry, because of its simple sample preparation requirement and high throughput. Since poliploidy is more common in plants, this method makes it ideal to estimate the ploidy level, too. It works by suspending cells in a stream of fluid and passing them by an electronic detection apparatus (wikipedia).  You usually get the results as ρg (pico grams) of DNA. To convert into basepairs (bp), which gives more realistic measure of DNA content, you can use these conversions

genome size (bp) = (0.978 x 109) x DNA content (pg)
so roughly,
1 pg = 978 Mb
