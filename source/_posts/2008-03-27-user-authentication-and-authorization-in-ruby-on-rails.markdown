---
layout: page
title: "User authentication and authorization in Ruby on Rails"
date: 2008-03-27 19:42
comments: true
sharing: true
footer: true
---
The system I am working on needs a good authentication (authn) and authorization (authz) system.   So far, I can't find anything that would work for me, so it looks like I'll have to build one.

### What I need

The ideal system would have the following features:

* Authn against multiple sources (LDAP, CAS, Local DB, etc.)
  * Authn would have an order that it would check the sources.
* An interface to set the sources and to set a mapping from Authn source information to local Authz permissions
* Authz at the controller or model level through the use of roles
* The Authz would have an understanding of an "owner" of a model item (if it has_one "owner") and be able to set permissions for the "owner".
* Users can be assembled into groups, and the group can be given a role.  User-level permissions trump Group-level permissions
* An interface to assign groups and roles, and to set permissions for the roles
* An interface to import users from the Authn source into the local app (so that an admin can assign roles/rights to users before they've ever logged in)
* An admin interface to allow users to be self-created (register now!) or only manually/authn created.

### What's out there

There's already an [acl_system2](http://aclsystem.rubyforge.org/) plugin out there that does some of this stuff. It does authentication against the local database, and controller-method level permissions.  But it doesn't have a concept of "owner".  So you couldn't easily set an object to be "editable by the owner or an admin".  It doesn't let you set permissions on the model level.  And it doesn't let you use any kind of GUI to change the permissions, it's all hard-coded. 

Next up is [restful_authentication](http://agilewebdevelopment.com/plugins/restful_authentication), it does just authn, and it doesn't auth against any external sources.  That doesn't mean it couldn't, but it would be nice if it did.  Perhaps it makes sense to use it as just the local authn mechanism, and roll other remote authn mechanisms separately and then put them all under an authn system.  Using the [role_requirement](http://code.google.com/p/rolerequirement/) plugin, you can also get roles. But it is still statically defined in the code, there's no UI for updating the role permissions during program runtime.

Also, [Redmine](http://redmine.org) has a pretty good authn/authz system that will authorize against an LDAP directory.  The way that it's organized, it looks like adding more Authz sources is possible.  However it only allows one Role per Person per Project.  That's not good enough for what I want.  I want a many to many user/role relationship.  It also doesn't have groups or an importer.  That is a problem for us, as we constantly want to add people to projects when they haven't yet logged in, and so we have to tell them:  log in, then tell us, then we can add you.  An importer from the LDAP server would be ideal.

Finally, there's a cool writeup on implementing [restful authentication with all the bells and whistles](http://www.railsforum.com/viewtopic.php?id=14216).  It's a great tutorial for getting authn/authz (using OpenID instead of LDAP) running.  However, it's not very modular and still has the the problem of grabbing users form the Authn source (that wouldn't have been possible with OpenID anyway, but could have been possible with LDAP).

### Goal

I'm going to make what I want, and then I'll stick it somewhere for the world to grab it if they want. 

