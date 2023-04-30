---
layout: post
title: PBS How to submit jobs that depend on previously submitted jobs
excerpt: "Submitting dependency jobs in PBS-Torque job scheduler"
date: 2014-04-11T15:21:00+00:00
modified: 2014-04-11T15:21:00+00:00
tags: [Arun Seetharam, PBS, Torque, bioinformatics, bash]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

To submit jobs one after the other (i.e., run second job after the completion of first), we can use `depend` function of `qsub`

First submit the firstjob, like normal

```bash
qsub first_job.sub
```

You will get the output (jobid#)

```bash
1234567.hpc
```

Second submit the second job following way,

```bash
qsub -W depend=afterok:1234567 second_job.sub
```

Both job will be queued, but second job won't start till the first job is finished

You can also automate this step using a simple bash script

```bash
#!/bin/bash
FIRST=$(qsub first_job.sub)
SECOND=$(qsub -W depend=afterok:$FIRST second_job.sub)
THIRD=$(qsub -W depend=afterok:$SECOND third_job.sub)
FOURTH=$(qsub -W depend=afterok:$THIRD fourth_job.sub)
```
