---
layout: page
title: "Rails, Nginx, OpenVZ, Mongrel Cluster & Mephisto"
date: 2008-03-26 23:05
comments: true
sharing: true
footer: true
---

Whew, that's a mouthful of a title, however, I think it's a good one.  I wanted to catch as many keywords as I could because I had a heck of a time getting all of my stuff running, I had to piece it together from goo stretched all over the internet.  But you're reading it now, and here's how I did it.
<!--more--> 
## Why

You can skip this if you want, but I thought a bit of "why" would be nice.  I am now doing full-time RoR development.  It seemed like a good idea to keep a blog about the problems I was encountering and how I solved them.  It is so often that my own problems are solved by other people doing just that... it seemed time to give back.

## Getting started

I started with a Debian Etch LAMPP(Linux Apache Mysql Postgresql Php) server that was running a bunch of vhosts(virtual hosts) with apache2.  I didn't want to disrupt anything already there (I had friends who shared the server with me), but Rails apps aren't intended to be virtual-hosted, and doing so doesn't work very well.

Ideally I wanted OpenVZ installed, then put each rails app in a virtual machine (VM).  Apache2 would proxy to ports 80 and 443 on the VMs.  Then, the VM would run a lightweight webserver and mongrel cluster.  I decided to run Nginx (pronounced engine-x) as the webserver over lighttpd based on this [a comparison I found](http://hostingfu.com/article/nginx-vs-lighttpd-for-a-small-vps).

So, to summarize my steps:

1. Install OpenVZ
2. Create a VM
3. Configure Apache2 to proxy to the VM as a vhost
4. Install Nginx and Rails on the VM
5. Install Mongrel

At this point, I have a VM that I can copy to create more rails apps in the future if I want to.

But for this VM, I want a blog, so I will lastly:

* Install Mephisto

I need to make sure that all of this works, so I am going to post a series of articles and then link to them from this article. Hopefully, at the end of it, it will take you less time than it took me.  Yay for the internet!
