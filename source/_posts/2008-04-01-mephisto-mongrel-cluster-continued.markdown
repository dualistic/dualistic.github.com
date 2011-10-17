---
layout: page
title: "Mephisto mongrel cluster (continued)"
date: 2008-04-01 13:55
comments: true
sharing: true
footer: true
---
So, in regards to "this problem":http://pol.llovet.name/2008/3/28/mephisto-mongrel-cluster-stops-responding, the server hasn't frozen in 10 hours or so, problem solved?

Here's what I did:

I had to install two packages:

<macro:code>
root@pol:/# apt-get install mysql-client libmysqlclient15-dev
</macro:code>

Then I was able to install the mysql gem with the config parameter:

<macro:code>
root@pol:/# gem install mysql -- --mysql-config=/etc/mysql/my.cnf
</macro:code>

You do have to be root (or use sudo) to run those commands. 

Then I restarted the mongrel cluster, and it's been running well so far.

Hopefully the problem is fixed!

