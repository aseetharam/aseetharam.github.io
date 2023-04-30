---
layout: post
title: Shortcuts for SSH hosts
excerpt: "adding the config file to make SSH connection easier"
date: 2018-10-05T15:21:00+00:00
modified: 2018-10-05T15:21:00+00:00
tags: [Arun Seetharam, shortcuts, config, bashrc, bioinformatics, bash]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

Are you tired of typing full length host-names while connecting via SSH? Do you frequently scp files from one server to another and have to lookup what the host-names are? Do you want to rsync between local and remote host easily with a a simple command? Then, read-on.

The hard-way:

```bash
# connect
ssh username@clustername.hostdomain.dept.edu
# scp
scp yourfile username@clustername.hostdomain.dept.edu:/path/to/destination/
# rsync
rsync -e 'ssh -c aes128-ctr' -rts your_folder username@clustername.hostdomain.dept.edu:/path/to/destination/
```

As you can see, if you have a bunch of hosts, it gets really hairy to retype them everytime you want to do any of these things.

The Solution:

Create a config file under the ~/.ssh directory, with the short name for these host-names. Then you can simply connect to the server by using the short name instead of the full host-name!

First, edit the file

```bash
vi ~/.ssh/config
Shortcuts for SSH hosts
```

and add the details:

```
Host sweet
  Hostname clustername.hostdomain.dept.edu
  User username
  ForwardX11 yes
Host sugary
  Hostname anothercluster.hostdomain.dept.edu
  User username
  ForwardX11 yes
```

Set permissions straight:

```bash
chmod 600 ~/.ssh/config
```

Now, have fun! the above commands can now be done using:

```bash
# connect
ssh sweet
# scp
scp yourfile sweet:/path/to/destination/
# rsync
rsync -e 'ssh -c aes128-ctr' -rts your_folder sweet:/path/to/destination/
```

You can read more about the config by opening the man page:

```bash
man ssh-config
```

Hope this trick will make your life little easier!
