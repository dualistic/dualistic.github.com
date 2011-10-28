---
layout: page
title: "Multi-OS Password Management"
date: 2008-06-09 15:10
comments: true
sharing: true
footer: true
---
It's a problem trying to keep track of all of your passwords.  I have a mac laptop, mac workstation at work, and a windows machine at home.  Add to that the myriad of websites, servers and services that I have users and passwords for.  It's a mess.  However, I managed to find a perfect solution...
<!--more--> 
On windows, my password manager of choice is [pwsafe](http://passwordsafe.sourceforge.net/), but it only offers command-line functionality for mac (and presumably linux, though I'm not sure).  On the Mac, I quite like Keychain, it is tightly integrated with the OS, but the passwords are locked away somewhere.  Supposedly you can leverage the @security@ executable and gain access to it, but then importing/exporting would be a total pain in the ass.

My searches had turned up an app called "Password Gorilla", but it kind of sucked, there appeared to be nothing for me!

Then I ran into [KeePassX](http://www.keepassx.org/), a password management package with binaries for all of the major OS's.  I installed it onto the mac: perfect.  It's exactly what I wanted.  There is some wonky UI issues (it forgets which password db you are using when you lock it), but it is totally workable.

I plan on using FUSE to create an ssh directory to one of my servers and store the binaries and password database on them.  So far so good!
