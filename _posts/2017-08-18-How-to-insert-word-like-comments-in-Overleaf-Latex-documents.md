---
layout: post
title: How to insert word like comments in Overleaf Latex documents
excerpt: "LaTex trick for collaborative writing!"
date: 2017-08-18T15:21:00+00:00
modified: 2017-08-18T15:21:00+00:00
tags: [Arun Seetharam, latex, comments, bioinformatics, bash]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

There is a `todo` package that can be used for this purpose. Simply add these lines to your document and you can leave comments with \todo commands.

```
\usepackage{geometry}
```


The above package will be used in most documents, so no need to add it if its already there

```
\usepackage[colorinlistoftodos]{todonotes}
```

by default it will be on right, to put it on left (if you don't have room beacuse of margins) use

```
\reversemarginpar
```

For commenting

```
\todo{this is an example comment}
```

Comments will be show as follows:
