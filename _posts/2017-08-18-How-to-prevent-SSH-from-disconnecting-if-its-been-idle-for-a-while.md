---
layout: post
title: How to prevent SSH from disconnecting if its been idle for a while
excerpt: "a trick in SSH to send signal a regular interval to keep your session alive"
date: 2017-08-18T15:21:00+00:00
modified: 2017-08-18T15:21:00+00:00
tags: [Arun Seetharam, SSH, alive, session, bioinformatics, bash]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

If you're using HPC, chances are that your University/Sysadmin has a policy about how long you can stay inactive with the established SSH connection. If you are frustrated with this automatic disconnections, here is a way to prevent this. You can either:

```
ssh -o "ServerAliveInterval 60" -X username@server.edu
```

or create a file in ~/.ssh/config with the following line:

```
ServerAliveInterval 60
```

This will enable ssh client keepalives. The above line will send an ssh keepalive every 60 seconds that will prevent network devices from considering the session as idle.

_Source_: [https://superuser.com/a/699680/173980](https://superuser.com/a/699680/173980)
