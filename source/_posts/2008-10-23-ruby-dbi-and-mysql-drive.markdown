---
layout: page
title: "ruby dbi and mysql driver"
date: 2008-10-23 19:41
comments: true
sharing: true
footer: true
---
If you want to do direct access to mysql (or other databse) you can use the dbi gem to do it.  But what the docs don't tell you is that you need to gem install a database driver also.  So if you want to use mysql, you do:

<pre>
$ sudo gem install dbi dbd-mysql
</pre>

There are other dbd's for different databases.

Also:
If you are getting this error:
<pre>
LoadError: dlopen(/Library/Ruby/Gems/1.8/gems/mysql-2.7/lib/mysql.bundle, 9): Library not loaded: /usr/local/mysql/lib/mysql/libmysqlclient.15.dylib
  Referenced from: /Library/Ruby/Gems/1.8/gems/mysql-2.7/lib/mysql.bundle
  Reason: image not found - /Library/Ruby/Gems/1.8/gems/mysql-2.7/lib/mysql.bundle
	from /Library/Ruby/Gems/1.8/gems/mysql-2.7/lib/mysql.bundle
	from /Library/Ruby/Site/1.8/rubygems/custom_require.rb:32:in `require'
	from (irb):1
	from /Library/Ruby/Gems/1.8/specifications/actionpack-1.13.6.gemspec:13
</pre>
Then you need to do this:
<pre>
sudo install_name_tool -change /usr/local/mysql/lib/mysql/libmysqlclient.15.dylib /usr/local/mysql/lib/libmysqlclient.15.dylib /Library/Ruby/Gems/1.8/gems/mysql-2.7/lib/mysql.bundle
</pre>
The last argument may need to be modified to match the "Referenced from" line from your error.

Test the fix by opening up irb and doing a require 'mysql'
