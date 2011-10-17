---
layout: page
title: "Mephisto mongrel cluster stops responding"
date: 2008-03-28 14:46
comments: true
sharing: true
footer: true
---

So, this site has only been up for a couple days, but I've had to restart the mongrel cluster twice.  Interestingly, it doesn't respond to a mongrel_rails cluster::restart, I have to do a cluster::stop, then a start to get it back up.  Then it works fine for a while.

Nginx seems to be motoring along just fine with no problems.

I am going to implement a change recommended "by this google code post":http://groups.google.com/group/MephistoBlog/browse_thread/thread/38d8db08beef1444.  The fix is to re-install the mysql gem with this option:

``` bash
 gem install mysql -- --mysql-config=/path/to/mysql_config
```

However, I don't have mysql installed on my virtual machine, since I have a centralized mysql server that I use for all of my virtual machines.  As a result, I don't have a mysql config to point to.  So, I installed just the mysql client with the following command: 

``` bash
root@pol:/# apt-get install mysql-client
```

This added 23M of extra stuff to the VM, but it also threw a my.cnf into the /etc/mysql dir.  

The next step was installing the mysql gem.  The hitch here is that I don't have a mysql server or dev stuff installed, so the gem doesn't want to compile.  I'm also curious how active record can work at all without the mysql gem installed...  I'm going to ask around for some more information and post it here when I've figure out where to proceed.

UPDATE: [I may have solved the problem here.](/blog/mephisto-mongrel-cluster-continued)

