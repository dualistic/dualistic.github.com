---
layout: page
title: "Redmine and Mercurial"
date: 2008-04-01 03:07
comments: true
sharing: true
footer: true
---
I was setting up a Mercurial repo for Redmine today.  As it turns out, Redmine chokes on initial repositories.  After creating a single file and checking it in as an "initial commit" ( I just touched the `.hgignore` file), it worked just fine.

I am, however, having trouble getting apache2+mercurial to work happily.  For some reason, the `hgwebdir.cgi` script isn't being run, it's just spitting out the text.  
<!--more--> 
To add to the problems, I tried running the script from the command-line (it's just a python script) and it spewed a bunch of errors (I think the problem is a lack of wsgi libraries, but I'm not sure).  If I do get it working, I'll mention what I did here.

About the Mephisto/mongrel cluster problem, it's still hanging every few hours.  I am at a loss as to why, and I don't want to just install things until it works.  I like my VMs to be lean, that's the whole point.  I tried poking the rails IRC channel about it, but didn't get any takers.

UPDATE:  [I finish this up here.](/blog/2008/04/01/mercurial-apache2-and-mod_rewrite/)
