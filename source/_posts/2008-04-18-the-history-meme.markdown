---
layout: page
title: "The History Meme"
date: 2008-04-18 17:02
comments: true
sharing: true
footer: true
---
[See Here.](http://diveintomark.org/archives/2008/04/15/history-meme)

```
chem69:ipm pol$ history | awk '{a[$2]++}END{for(i in a){print a[i] " " i}}' | sort -rn | head
123 cd
118 ls
97 hg
19 vim
19 script/server
17 rake
12 script/generate
12 cp
8 ssh
7 script/plugin
```
