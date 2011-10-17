---
layout: page
title: "apache2 mod_rails recipe"
date: 2008-10-06 01:12
comments: true
sharing: true
footer: true
---
I recently had to write a recipe to set up instances of mod_rails apps for apache2.  The goal of this process is to have a server that has apps hosted at http://server/app1, http://server/app2, etc.

The recipe is going to be used to create a puppet script, but it could also be boiled down to a ruby or shell script to easily create app instances for anyone's use.

Assumptions and Server Setup

* All of this is done as sudo or root
* The OS has apache2, mysql, sqlite3, ruby, rubygems, git-core and
subversion installed
* The gems sqlite3-ruby, mysql, rails, capistrano are installed

If apache2 is not yet configured with mod_rails, do the following:

* Create /etc/apache2/mods-available/mod_rails.conf and put the following in it:

<code>
 # Ruby Passenger Config
 LoadModule passenger_module /usr/lib/ruby/gems/1.8/gems/passenger-2.0.3/ext/apache2/mod_passenger.so
 PassengerRoot /usr/lib/ruby/gems/1.8/gems/passenger-2.0.3
 PassengerRuby /usr/bin/ruby1.8
</code>

* Symlink it into enabled:  ln -s /etc/apache2/mods-available/mod_rails.conf /etc/apache2/mods-enabled/mod_rails.conf
* Restart apache2

Optionally make Ubuntu's username policy more sensible (allow underscores)

<code>$ echo "NAME_REGEX=\"[a-z_][a-z0-9_-]*[$]?\"" >> /etc/adduser.conf</code>

h3. Creating a ModRails Application @http://some.host.name/appname

p. create the user, the appname will be the username:

<code>$ adduser <username> (use pwgen or something for the password)</code>

p. create a stub rails app:

<code>$ rails /home/<username>/rails/testapp</code>

p. Link the testapp to the "static" application location

<code>$ ln -s /home/<username>/rails/testapp /home/<username>/rails/app</code>

p. Link the "static" application location into the servers webroot

<code>$ ln -s /home/<username>/rails/app/public /var/www/<username></code>

p. Create the apache2 conf for the rails app

<code>$ echo "RailsBaseURI /<username>" > /etc/apache2/sites-available/<username></code>

p. Tell apache2 to actually use the app

<code>$ ln -s /etc/apache2/sites-available/<username> /etc/apache2/sites-enabled/<username></code>

p. Make sure that the new user has ownership of the files

<code>$ chown -R /home/<username>/rails <username>.<username></code>

p. Restart apache2 and check to see if it works

<code>
$ apache2ctl graceful
$ wget http://localhost/<username> --server-response --spider # this should return 200 OK
</code>

p. Create a mysql user and databases for the app

<code>
 $ for db in production test development; do mysql -u root -pMYSQLPASS -e "grant all privileges on <username>_$db.* to '<username>'@'localhost' identified by '<userpassword>'; create database <username>_$db;"  done
</code>

p. Copy template capistrano deployment files into their home/<username>/rails directory

At this point the application will be available running at http://hostname/<username>

With one of the deploy.rb files, a savvy rails user can deploy their application to the server with capistrano

A less savvy rails user can connect via ssh and checkout their their rails app to someplace in their home, then symlink it to the ~/rails/app directory
