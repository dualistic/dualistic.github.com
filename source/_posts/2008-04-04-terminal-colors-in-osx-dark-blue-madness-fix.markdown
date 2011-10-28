---
layout: page
title: "Terminal Colors in OSX (Dark Blue Madness Fix)"
date: 2008-04-04 17:32
comments: true
sharing: true
footer: true
---

Editor Note:  Just use [iTerm](http://www.iterm2.com/).  It works great.

This is posted in [other](http://www.infinitered.com/blog/?p=24) [places](http://ciaranwal.sh/2007/11/01/customising-colours-in-leopard-terminal), but I want to make sure that it doesn't get lost to the wilds of the interwebs.

The default black background themes in Terminal suck if you are doing ANSI colors in the terminal.  I like colors in the term, but the dark-blue is aneurysm inducing.  So, after futzing with the defaults a bunch, I decided to see if the internet had any solutions.  By his noodlyness, they did.  
<!--more--> 
So, here's what needs doing:

1. Download the latest [SIMBL](http://www.culater.net/software/SIMBL/SIMBL.php) and install it
* Create a `~/Library/Application Support/SIMBL/Plugins` directory
* Download the [TerminalColors](http://ciaranwal.sh/files/TerminalColours.bundle.zip) SIMBL bundle
* Unzip it and copy it to the directory we just created
* Close Terminal
* Optionally, download and run [InfiniteRed's theme](http://www.infinitered.com/settings/IR_Black.terminal.zip)
* If that didn't start Terminal, then start it up.
* In the settings there is a "more" button with colors that map to ANSI terminal colors.  Change them to your liking!

I am going to try to make a terminal theme that matches my favorite TextMate theme, Succulent ([screenshot](http://wiki.macromates.com/Themes/UserSubmittedThemes), [download](http://www.fearoffish.co.uk/assets/2006/9/26/Succulent_1.tmTheme))
