---
layout: post
title: "I know we can do better, Ruby (a simple setup in 8 excruciating steps)"
date: 2011-10-27 22:05
comments: true
categories: [ruby, rvm, soap, pita]
---
I love Ruby, but it can be a total pain in the ass. Not the language, mind you, but the ecosystem.  And the crazy thing is, the Rubygems+RVM setup is basically unparalleled in the development world for flexibility and ease of use.  I really can't think of any other dev environment that is half as cool. So, what's wrong?  Well, let's go on a little journey... 

## A Simple Problem ##

As mentioned in a [prior post](/blog/2011/10/26/soaping-with-ruby-and-php/), I am doing some messing around with SOAP.  As it happens, I am moving from my development area to try this in a server machine.  No problem.

Note: I am going to include every hiccup I ran into, even those that aren't the fault of the Ruby ecosystem, just to be thorough.  I am just tracing the steps that a complete newb would.

### Step 1: RVM ###

This is a fresh install of Ubuntu, so I go to the RVM site, copy the install command and paste it into my terminal:

``` bash install rvm (try 1)
$ bash < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer )
The program 'curl' is currently not installed.  You can install it by typing:
sudo apt-get install curl
```

**Hitch #1:** Curl isn't installed on fresh Ubuntu.<br /> **Solution:** `sudo apt-get install curl`

Next try:

``` bash install rvm (try 2)
$ bash < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer )
bash: line 154: git: command not found
bash: line 156: git: command not found

ERROR: Unable to clone the RVM repository, attempted both git:// and https://
```
**Hitch #2:** Git isn't installed on fresh Ubuntu.<br /> **Solution:** `sudo apt-get install git-core`

Note: I did a quick check of the RVM docs, and it doesn't seem to mention anywhere that git/curl may not be available and what to do about it if they aren't.

``` bash install rvm (try 3)
$ bash < <(curl -s https://raw.github.com/wayneeseguin/rvm/master/binscripts/rvm-installer )
Initialized empty Git repository in /home/pol/.rvm/src/rvm/.git/
...SNIP...
# For JRuby, install the following:
  jruby: /usr/bin/apt-get install curl g++ openjdk-6-jre-headless
  jruby-head: /usr/bin/apt-get install ant openjdk-6-jdk

# For IronRuby, install the following:
  ironruby: /usr/bin/apt-get install curl mono-2.0-devel
```

Success! this point RVM installs cleanly, and after logging in/out (or `source ~/.bashrc`), RVM is ready to go.  However, it is of note that the WALL OF TEXT that RVM spits out is exceptionally daunting.  It would be nice if the last thing shown was generally relevant (like, hey, you need to restart your shell), not here's what you need for IronRuby.  I know, I know, RTFM, but... it's a thing. 

### Step 2: Ruby ###

I need ruby 1.9.2, so let's grab that.

``` bash install ruby 1.9.2
$ rvm install ruby-1.9.2-p290
Installing Ruby from source to: /home/pol/.rvm/rubies/ruby-1.9.2-p290, this may take a while depending on your cpu(s)...

ruby-1.9.2-p290 - #fetching 
ruby-1.9.2-p290 - #downloading ruby-1.9.2-p290, this may take a while depending on your connection...
...SNIP...
Install of ruby-1.9.2-p290 - #complete
```
Great! Mission accomplished, everything built cleanly.

### Step 3: Create a Gemset ###

Gemsets are good practice, so let's make one:

``` bash create and use a gemset
$ rvm gemset create webofscience
'webofscience' gemset created (/home/pol/.rvm/gems/ruby-1.9.2-p290@webofscience).
$ rvm gemset use webofscience
Using /home/pol/.rvm/gems/ruby-1.9.2-p290 with gemset webofscience
```
Awesome.  Again, everything going smoothly (yay we <3 ruby!) It's worth mentioning here that at this point I am getting warm and fuzzy feelings about Ruby+RVM.  This is how it *should be*.

### Step 4: Install the Gems (try 1) ###

Last step before pulling out the editor, get the gems:

``` bash install the gems
$ gem install savon handsoap
ERROR:  Loading command: install (LoadError)
    no such file to load -- zlib
ERROR:  While executing gem ... (NameError)
    uninitialized constant Gem::Commands::InstallCommand
```
Wait, WTF?  There haven't been any errors or anything, what is this?

**Hitch #3:** Mysterious "no such file to load -- zlib" ERROR.<br /> **Solution:** At this point the user will need to google.  If they have good google-fu, they will find the stack-overflow solution (there are other ways to solve this, but the rvm one is the cleanest).

### Step 5: Remove Ruby, add zlib, redo steps 2 & 3 ###

Ok, so, a minor hitch, just add a lib, change the rvm ruby install command a bit, and get caught up again.

``` bash remove ruby, add zlib, and recompile
$ rvm gemset delete webofscience
WARN: Are you SURE you wish to remove the entire gemset directory 'webofscience' (/home/pol/.rvm/gems/ruby-1.9.2-p290@webofscience)?
(anything other than 'yes' will cancel) > yes
$ rvm remove ruby-1.9.2-p290
Removing /home/pol/.rvm/src/ruby-1.9.2-p290...
Removing /home/pol/.rvm/rubies/ruby-1.9.2-p290...
Removing ruby-1.9.2-p290 aliases...
Removing ruby-1.9.2-p290 wrappers...
Removing ruby-1.9.2-p290 environments...
Removing ruby-1.9.2-p290 binaries...
$ rvm pkg install zlib
Fetching zlib-1.2.5.tar.gz to /home/pol/.rvm/archives
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100  531k  100  531k    0     0   318k      0  0:00:01  0:00:01 --:--:--  371k
Extracting zlib-1.2.5.tar.gz to /home/pol/.rvm/src
Configuring zlib in /home/pol/.rvm/src/zlib-1.2.5.
Compiling zlib in /home/pol/.rvm/src/zlib-1.2.5.
Installing zlib to /home/pol/.rvm/usr
$ rvm install 1.9.2-p290 -C --with-zlib-dir=$rvm_path/usr
Installing Ruby from source to: /home/pol/.rvm/rubies/ruby-1.9.2-p290, this may take a while depending on your cpu(s)...
...SNIP...
Install of ruby-1.9.2-p290 - #complete
$ rvm use ruby-1.9.2-p290
Using /home/pol/.rvm/gems/ruby-1.9.2-p290
$ rvm gemset create webofscience
'webofscience' gemset created (/home/pol/.rvm/gems/ruby-1.9.2-p290@webofscience).
$ rvm use ruby-1.9.2-p290@webofscience
Using /home/pol/.rvm/gems/ruby-1.9.2-p290 with gemset webofscience
```
Rockin.  Minor misstep, let's keep on rollin'.

### Step 6: Install the Gems (try 2) ###

Second time's the charm!  You'd think...

``` bash install the gems (try 2)
$ gem install savon handsoap
Fetching: builder-3.0.0.gem (100%)
Fetching: nori-1.0.2.gem (100%)
Fetching: rack-1.3.5.gem (100%)
Fetching: httpi-0.9.5.gem (100%)
Fetching: nokogiri-1.5.0.gem (100%)
Building native extensions.  This could take a while...
ERROR:  Error installing savon:
	ERROR: Failed to build gem native extension.

        /home/pol/.rvm/rubies/ruby-1.9.2-p290/bin/ruby extconf.rb
checking for libxml/parser.h... no
-----
libxml2 is missing.  please visit http://nokogiri.org/tutorials/installing_nokogiri.html for help with installing dependencies.
-----
*** extconf.rb failed ***
...SNIP...
Successfully installed handsoap-1.1.8
1 gem installed
Installing ri documentation for handsoap-1.1.8...
Installing RDoc documentation for handsoap-1.1.8...
```
Ok, missing libxml2.  Again, would have been nice to know this earlier... but this is more like the git/curl situation, it's an OS thing.  

**Hitch #4:** Nokogiri gem has a compilation error.<br /> **Solution:** After googling around, the nokogiri docs say that on ubuntu, you need a couple more libs: `sudo apt-get install libxslt-dev libxml2-dev`

After installing the libs, the gems finally install cleanly:

``` bash install the gems (try 3)
$ gem install savon handsoap
Building native extensions.  This could take a while...
Fetching: wasabi-2.0.0.gem (100%)
Fetching: gyoku-0.4.4.gem (100%)
...SNIP...
Installing RDoc documentation for savon-0.9.7...
Installing RDoc documentation for handsoap-1.1.8...
```
Great!  Gems are installed, everything is ready to rock n' roll.

### Step 7: Run the code ###

So, the code is super trivial ([gist](https://gist.github.com/1321660) if you want to see), let's run it:

``` bash run the code!
$ ruby wos.rb
/home/pol/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require': no such file to load -- openssl (LoadError)
	from /home/pol/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require'
	from /home/pol/.rvm/gems/ruby-1.9.2-p290@webofscience/gems/httpi-0.9.5/lib/httpi/auth/ssl.rb:1:in `<top (required)>'
	from /home/pol/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require'
	from /home/pol/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require'
	from /home/pol/.rvm/gems/ruby-1.9.2-p290@webofscience/gems/httpi-0.9.5/lib/httpi/auth/config.rb:1:in `<top (required)>'
	from /home/pol/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require'
	from /home/pol/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require'
	from /home/pol/.rvm/gems/ruby-1.9.2-p290@webofscience/gems/httpi-0.9.5/lib/httpi/request.rb:2:in `<top (required)>'
	from /home/pol/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require'
	from /home/pol/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require'
	from /home/pol/.rvm/gems/ruby-1.9.2-p290@webofscience/gems/savon-0.9.7/lib/savon/client.rb:1:in `<top (required)>'
	from /home/pol/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require'
	from /home/pol/.rvm/rubies/ruby-1.9.2-p290/lib/ruby/site_ruby/1.9.1/rubygems/custom_require.rb:36:in `require'
	from /home/pol/.rvm/gems/ruby-1.9.2-p290@webofscience/gems/savon-0.9.7/lib/savon.rb:3:in `<top (required)>'
	from <internal:lib/rubygems/custom_require>:33:in `require'
	from <internal:lib/rubygems/custom_require>:33:in `rescue in require'
	from <internal:lib/rubygems/custom_require>:29:in `require'
	from wos.rb:1:in `<main>'
```
Can I get another WTF?  Everything installed clean!  Ruby, RVM, Gems, everything... why is this breaking?  

**Hitch #5:** Running the code throws a "no such file to load -- openssl" error.<br /> **Solution:** Google again to the rescue; more specifically stack-overflow to the rescue.  This solution is similar to the first: use rvm to add the missing libs to ruby. Awesome, we get to re-compile again.

### Step 8: Remove Ruby, add openssl and iconv, redo steps 2, 3, 6 & 7 ###

We've had some practice, so let's do it again, though the stack-overflow said to install iconv in addition to openssl.  Since we won't get *any* information in advance as to whether or not iconv is required, let's just do it now.  Also, don't forget the original zlib compilation that we did!

``` bash remove ruby, add openssl, and recompile
$ rvm gemset delete webofscience
WARN: Are you SURE you wish to remove the entire gemset directory 'webofscience' (/home/pol/.rvm/gems/ruby-1.9.2-p290@webofscience)?
(anything other than 'yes' will cancel) > yes
$ rvm remove ruby-1.9.2-p290
Removing /home/pol/.rvm/src/ruby-1.9.2-p290...
Removing /home/pol/.rvm/rubies/ruby-1.9.2-p290...
Removing ruby-1.9.2-p290 aliases...
Removing ruby-1.9.2-p290 wrappers...
Removing ruby-1.9.2-p290 environments...
Removing ruby-1.9.2-p290 binaries...
$ rvm pkg install openssl
Fetching openssl-0.9.8n.tar.gz to /home/pol/.rvm/archives
  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
100 3681k  100 3681k    0     0   560k      0  0:00:06  0:00:06 --:--:--  704k
Extracting openssl-0.9.8n.tar.gz to /home/pol/.rvm/src
Configuring openssl in /home/pol/.rvm/src/openssl-0.9.8n.
Compiling openssl in /home/pol/.rvm/src/openssl-0.9.8n.
Installing openssl to /home/pol/.rvm/usr
$ rvm install 1.9.2 -C --with-openssl-dir=$rvm_path/.rvm/usr,--with-iconv-dir=$rvm_path/usr,--with-zlib-dir=$rvm_path/usr
Installing Ruby from source to: /home/pol/.rvm/rubies/ruby-1.9.2-p290, this may take a while depending on your cpu(s)...
...SNIP...
Install of ruby-1.9.2-p290 - #complete
$ rvm use ruby-1.9.2-p290
Using /home/pol/.rvm/gems/ruby-1.9.2-p290
$ rvm gemset create webofscience
'webofscience' gemset created (/home/pol/.rvm/gems/ruby-1.9.2-p290@webofscience).
$ rvm use ruby-1.9.2-p290@webofscience
Using /home/pol/.rvm/gems/ruby-1.9.2-p290 with gemset webofscience
$ gem install savon handsoap
Fetching: builder-3.0.0.gem (100%)
...SNIP...
Installing RDoc documentation for savon-0.9.7...
Installing RDoc documentation for handsoap-1.1.8...
$ ruby wos.rb
D, [2011-10-27T23:30:14.123847 #23626] DEBUG -- : HTTPI executes HTTP GET using the net_http adapter
...
```
And finally, it all works.

## Summary of Issues ##

1. (minor) RVM doesn't tell you which packages you need in order to use their install scripts
2. RVM doesn't check for required libs necessary to run rubygems, so everything will compile, but you may still have a nonfunctional system.
3. RVM also doesn't let you know which optiional libs might be needed by various ruby gems, and the gems don't tell you if it is just a system lib you are missing, or something that needs to be compiled into Ruby.
4. Gems that require additional compiled ruby functionality don't check to see if it is available when the gem is installed, it just fails at runtime.

The upshot is that while this is annoying for an experienced user, it could represent a brick wall for a new user.  This should be a straightforward and easy process. And I get the feeling that if I had removed the RVM variable, it would have made the problem even harder to solve because recompiling is harder when you have installed from apt.

I love ruby, but this is kind of embarrassing.  What can we do to make this better?
