---
layout: page
title: "Projects"
date: 2011-10-17 01:43
comments: true
sharing: true
footer: true
---

## Current Foci ##

### [Yogo](https://github.com/yogo) ###

The Yogo Project.  Yogo is a set of ruby libraries for building scientific data management applications. It is a data management framework that has been used to build some of the other software that is listed here. It is currently in development.

As a note, this project began its life many years ago as Neurosys.  It was thought that the name should change, so I offered Yogo (we were doing the software in Ruby, and the Yogo is a sapphire unique to Montana).  It stuck.  I really dislike the acronym-plague that saturates grant-funded projects.  It drives me *crazy*.  I suffer through it, but make every effort to name my projects without using acronyms.

### [VOEIS](https://github.com/yogo/VOEIS) ###

The Virtual Observatory and Ecological Informatics System.  VOEIS is a Yogo Framework application that is designed to manage Ecological data.  It is funded through NSF EPSCoR and is currently in development.  


### Evolution: The Game ###

A four player, team-based, tactical card game.  The teams are bridge-style, cooperative but without full knowledge of each other's situation.  Table-talk is prohibited, team members must signal information to their partner through their plays. 

This is a full-on physical game, no code involved.  We (my partner is an artist) are in the art/assembly stage trying to get all of the assets finished for a first prototype. 

### [Dualistic.com](https://github.com/dualistic/dualistic.github.com) ###

This website. It's built with [Octopress](http://octopress.com) which is a fabulous obsessively-designed static site generator. I love the heck out of it and it has been a joy to work with. 

Dualistic.com started long ago as a php site (which I have plans to import at some point), then I thought that I should use it for other things, and I built a [Mephisto](http://www.mephistoblog.com/) blog at pol.llovet.name (no longer there).  This never felt quite right to me, so I started on a [Sinatra](http://sinatrarb.com) app with a custom blog-thing.  It was fun to write, but in the midst of development I happened upon Octopress.  At the time I was considering how best to integrate code snippets and was thinking I was just going to have to use embedded gists. Octopress does it the way I would have done it if I had the time and persistence to do it properly.  Plus, I could store my posts in git, and revision them.  Add to that, I can host my gorgeous static site at github (combining two of my favorite tools, git and github).  Sold.  Done.  In the bag. 

## Will Get To Any Day Now... ##


### [Outliner](https://github.com/pol/outliner) ###

A simple outliner written in Ruby to make outlining papers easier. Seriously, here's what I wanted:

1. An editable outline
2. Collapsable sections

That's it.  And yet I couldn't find anything that worked.  There was really really heavy solutions (I don't need anything huge), and then things that didn't quite work (like Pages or Word).  I was thinking to myself, *you know, Textmate code folding would work great.*  So, I started writing a nested outline with textmate, using Ruby for the syntax (though python may have worked too, I know Ruby better and blocks are handy).  As I happily outlined, it occurred to me that it would take very little to make my fake ruby into real ruby that could render into an html page with javascript collapsing.  And there you have it.  

It is rough, I should have tests and probably gemify it.  There's no reason it couldn't be a standalone thing. 

### [FileDrop](https://github.com/pol/filedrop) ###

Simple file upload application (needs updates badly).  This was a fun small rails app to fit a use-case that was surprisingly unfilled.  Just a simple app that users could log into, upload a big file, and get back a unique link that they could send to someone to download the file.  The file would get deleted in a few days.  

It has been used a number of times by people who needed it, so it totally worked.  However, I am not going to develop it into what it should/could be since I found another application that does what I need and better. [FileSender](http://filesender.org) is that app.  It's heavier weight, but it has the enterprisey things I need.  Plus it has some code that allow much larger files.  I am in the process of creating a [Chef](http://www.opscode.com/chef/) recipe for a FileSender server, and will certainly blog about the results.  I expect that we (the [Research Computing Group](http://rcg.montana.edu)) will also create and host a VM image for other institutions to use should they need a similar service.

## Not Working On, But Notable ##

### [Crux](https://github.com/yogo/crux) ###

The Crux Experiment Management system.  Crux is notable in that it is a Yogo Framework application that used experimental protocols as a way to get scientists to describe their data model.  This was done through using the KEfED protocol description system.  KEfED breaks the protocol into variables (independent and dependent), actions, objects, conditionals, forks and branches. Crux then took the protocol graph, and sent it to the Yogo system to build into a database. 

Getting scientists to describe their data models is fairly difficult.  Scientists are not DBAs (nor should they be).  However, they do understand their protocol. This turned out to be a powerful way to communicate the data model.  Fun project.  This was funded by Michael J. Fox Foundation for Parkinson's Research. 

### FAD ###

The faculty activity database.  A database for faculty annual review data.  It sorely needs to be redesigned and reimplemented, but I don't have the time or bones to do it.  It's a straight up Rails app, but it should be a Yogo app (or at the very least use a nosql db like MongoDB).  The problem is totally tractable, but there aren't any good products on the market for doing faculty assessment.  There are many half-assed ones, but nothing like a slam-dunk (even in the enterprise market).

