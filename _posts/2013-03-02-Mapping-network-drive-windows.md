---
layout: post
title: Mapping network drives in Windows
excerpt: "hack for mapping multiple network drives on the same machine"
date: 2013-03-02T19:38:00+00:00
modified: 2013-03-02T19:38:00+00:00
tags: [Arun Seetharam, hack, windows8, network, mapping]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

I have connections to so many servers and it is painful to transfer files from those servers to local drive to edit/modify them. So I normally map them to my computer so that I can directly open them without saving them first. But yesterday while I was helping my colleague to map her drives to her computer we noticed that we could map only one drive from a server. When attempted to map another drive it gave an error:

![error](/images/p2-error.png)


The network folder specified is currently mapped using a different user name and password. To connect using different username and password, first disconnect any existing mappings to this network share


It was clearly an erroneous error message because she had never connected that drive before. Finally, we figured out that it was a bug that didn't let us map. The solution was easy, if you have signed in using different credentials for the server, you don't have to sign in every time you map another drive from the same server. It was as simple as that!

I hope if any of you had the same problem, this might help!
