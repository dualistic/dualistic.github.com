---
layout: page
title: "Applications with Multiple Authn Sources"
date: 2008-04-17 17:11
comments: true
sharing: true
footer: true
---
I've previously written that I want a system that can handle multiple authentication sources.  After hammering on it a bit, there are some pitfalls that exist with this kind of scenario.  

First, a recap.  I want a system that can authenticate a user from multiple sources.  The sources will be organized into a priority list.  So, for example, if User1 asks for a page that is restricted, first I check to see if they have a pubcookie or known CAS/SSI(Central Authentication Server/Single Sign On) cookie.  If so, I check to see if they have a user in the system.  If so, then I log them in.  If not, then I create one (noting the authn source) and log them in.  If they don't have a cookie, then I send them to a login page.  With their username and password credentials, I try to log them into the list of sources.  If there is only one username that matches that user in my system, I try to log them into that first, otherwise I progress through the list.  If a remote authentication server verifies their username/email/password as a valid user, i check for a local user.  If it exists, I log them in, if not i create a local user, note their authn source, and log them in.

The ramifications of this is that there are allowed to be users with the same username.  However, the  tuple of username, password and authn source is unique.  there will only be one Foo with the password Blarg which is authenticated with my company LDAP server.  There might be a local user with the username Foo, but I'm assuming that everyone will have different passwords.  If someone did use the same username and password as another person, and they used a higher priority authn service, they would always be the person that would log in as that user.

This is not ideal, I will think on it further.
