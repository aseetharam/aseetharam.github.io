---
layout: post
title: Highlight words enclosed within parenthesis in MS Word
excerpt: "A regex like trick in MS Word to speed up your work!"
date: 2014-04-16T15:21:00+00:00
modified: 2014-04-16T15:21:00+00:00
tags: [Arun Seetharam, highlight, word, microsoft, bioinformatics, bash]
categories: [bioinformatics]
comments: true
#image:
#  feature: image.jpg
#  credit: Arun Seetharam
#  creditlink: http://aseetharam.github.io
---

Office 2013 offers robust regular expression matching, that will be very handy to do some basic stuff. One example, I was recently given a 10 page document to edit and incorporate citations. They wanted me to look for the authors (citations) within the text and add correct reference in the "references" section. One problem: there were way too many citations! It was hard for me to distinguish authors and normal text. I decided to highlight all the authors using regular expressions. This is how I did it:




Here the first `"\"` is escape character, this tells `Word` to treat the next character `"("`, as-is i.e., find a opening parenthesis in the document. Next, `"(*)"` tells `Word` to find one or more words after the opening parenthesis, and finally `"\)"` the search pattern ends when it encounters closing parenthesis (requires an escape character). So basically it matches anything within the parenthesis.

Note that you need to have `"Use wildcards"` checked. For the next part (to highlight), simply click on `"Format"` button and select `"Highlight"`. When done, just click on `"Replace All"`. You will have all the text in the document, within the parenthesis, highlighted! You can change `"\("` and `"\)"` to other things as well. For eg., `"\{"` and  `"\}"` for text within the curly braces, so on.
