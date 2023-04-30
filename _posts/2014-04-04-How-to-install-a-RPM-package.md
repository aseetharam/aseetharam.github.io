---
layout: post
title: How to install a RPM package
excerpt: "simple trick to open a RPM package and install the programs if you can't find the source"
date: 2014-04-04T15:21:00+00:00
modified: 2014-04-04T15:21:00+00:00
tags: [Arun Seetharam, rpm, install, bioinformatics, bash, red-hat]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

If you have access to a large computing cluster (HPC), chances are you will be running Red Hat Enterprise Linux operating system. Also, you will be a regular user without privileges to install any programs system-wide. Sometimes it might be necessary to install some programs. If you have a good support team running the cluster, installation will be as easy as sending them a request email. But, if you are like me, maintaining programs for your group, then you'll more likely end up installing one or the other package for the users in your group. With RHEL OS, some of the packages are only found as RPM packages (all centOS RPM packages are compatible with RHEL). You can download them (either source or binaries) from various locations such as RPM find, PKGS etc., To install them follow these steps:

```bash
rpm2cpio package.rpm |cpio -idv
# will extract the RPM into different files
# you will most likely find a tar.gz file, and sometimes few patch files
# first extract the tar.gz file and cd into it
tar -xzf package.tar.gz
cd package
# apply patch
patch -Np1 -i ../name_of_the_file.patch
# install as normal
./configure --prefix=/path/to/install/programs
make
make install
```
