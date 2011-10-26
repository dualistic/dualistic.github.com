---
layout: page
title: "openvz and mod_rails"
date: 2008-05-09 16:29
comments: true
sharing: true
footer: true
---
Setting it up was pretty easy, though I had to triple my memory allocation over what I was using with nginx+mongrel.  

* I created a new virtual machine, but with more memory than usual: `PRIVVMPAGES="150000:300000"`
* I `vzctl enter <the new machine>`
* as root:

```
$ apt-get ruby ruby1.8-dev rubygems apache2 apache2-mpm-prefork build-essential vim
$ gem install fastthread 
$ gem install rake (i have rails frozen into my apps, otherwise I would have installed it also)
$ gem install passenger
```

* for some reason, my binaries were not linked, nor the rubygems path exported, so: 

```
$ ln -s /var/lib/gems/1.8/bin/* /usr/bin (heavy handed, but it works)
```

* run `passenger-install-apache2-module` and follow the instructions

after starting apache2, i was able to see my site up and running.  yay!
