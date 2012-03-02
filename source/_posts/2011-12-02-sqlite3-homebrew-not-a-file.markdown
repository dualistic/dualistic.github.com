---
layout: post
title: "Sqlite3 Homebrew 'Not a File'"
date: 2011-12-02 12:31
comments: true
categories: 
published: false
---

So, one of the expected libraries is buried in a directory (?!?), and you need to move it out. 

```
~/Projects/Ruby/yogo/yogo-project ∴ irb
Loaded ~/.irbrc
ruby-1.9.2-p290 :001 > require 'sqlite3'
LoadError: dlopen(/Users/pol/.rvm/gems/ruby-1.9.2-p290@yogo-project/gems/sqlite3-1.3.4/lib/sqlite3/sqlite3_native.bundle, 9): Library not loaded: /usr/local/lib/libsqlite3.0.8.6.dylib
  Referenced from: /Users/pol/.rvm/gems/ruby-1.9.2-p290@yogo-project/gems/sqlite3-1.3.4/lib/sqlite3/sqlite3_native.bundle
  Reason: no suitable image found.  Did find:
	/usr/local/lib/libsqlite3.0.8.6.dylib: not a file
	/usr/local/lib/libsqlite3.0.8.6.dylib: not a file - /Users/pol/.rvm/gems/ruby-1.9.2-p290@yogo-project/gems/sqlite3-1.3.4/lib/sqlite3/sqlite3_native.bundle
	from /Users/pol/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require'
	from /Users/pol/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require'
	from /Users/pol/.rvm/gems/ruby-1.9.2-p290@yogo-project/gems/sqlite3-1.3.4/lib/sqlite3.rb:6:in `rescue in <top (required)>'
	from /Users/pol/.rvm/gems/ruby-1.9.2-p290@yogo-project/gems/sqlite3-1.3.4/lib/sqlite3.rb:2:in `<top (required)>'
	from /Users/pol/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:59:in `require'
	from /Users/pol/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:59:in `rescue in require'
	from /Users/pol/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:35:in `require'
	from (irb):1
	from /Users/pol/.rvm/rubies/ruby-1.9.2-p290/bin/irb:16:in `<main>'
```

Do this

```
~ ∴ cd /usr/local/lib
/usr/local/lib ∴ mv libsqlite3.0.8.6.dylib libsqlite3.0.8.6.dylib.wtf
/usr/local/lib ∴ cd libsqlite3.0.8.6.dylib.wtf 
/usr/local/lib/libsqlite3.0.8.6.dylib.wtf ∴ mv libsqlite3.0.8.6.dylib ..
/usr/local/lib ∴ 
```

Then you get this:

```
ruby-1.9.2-p290 :002 > require 'sqlite3'
 => true 
```



