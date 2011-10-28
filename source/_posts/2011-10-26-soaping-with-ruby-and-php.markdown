---
layout: post
title: "SOAPing With Ruby and PHP"
date: 2011-10-26 21:10
comments: true
categories: [soap, ruby, php, prototype]
---
So, there are some folks on campus who would like to have their faculty's journals listed on their faculty pages. This is certainly doable with some web-scraping, but the Web of Science (Web of Knowledge, actually) has an API, and the folks I have talked to have secured access to said API. 

Unfortunately, it is a SOAP API, something which I know nothing about. But, hey, how hard can it be?

My plan is: learn and prototype with ruby, and then fool around a little with PHP (to make sure it works), then make a bid to do the work (and include my already spent time :P ). 

### SOAPed up Ruby ###

My first stop for a library search is, of course, the excellent [Ruby Toolbox](http://www.ruby-toolbox.com).  They list [three options](https://www.ruby-toolbox.com/categories/soap): 

* [Savon](http://rubygems.org/gems/savon)
* [Handsoap](http://github.com/troelskn/handsoap)
* [Serviceproxy](http://github.com/jeremydurham/serviceproxy)

The searching also indicated that [SOAP4R](http://rubygems.org/gems/soap4r) was the original ruby SOAP library, but that it also is slow, old, and nobody likes it.  I decided to try first with Savon because it was first.

There should be a gist here:

{% gist 1321660 %}

...later...

### Initial thoughts: ###

* Auth info is sent in the header.  This seems sensible.  The user/pass is base64 encoded, appended to "Basic ", and then set in an "Authorization" header.  Fine.  Annoyingly, the ruby `Base64.encode64()` method appends an \n to the encoded string so you have to `encode64(string).strip` or Mr. SOAP API will throw a 400. Anyway, our access is auth'ed via IP, so this doesn't matter and I never did get to see what a successful user/pass auth looked like.
* I have to pretend to build a cookie and send headers on my http requests as though I was holding a cookie, even though I am just pretending. The cookie games feel awkward.
* The ?wsdl business is pretty neat.  I like that the SOAP endpoints can tell you what api calls they respond to. I need to look at this more, can they tell you how to properly form a particular API call?  I mean, just knowing that you can do a :search doesn't mean you know the format of the query...

This is doable, and it shouldn't even be that hard.  I just need to:

1. make sure that the school has access, because it is locked down by IP
2. figure out the query syntax
3. prototype the ruby calls to sort out the query and parse the response strings
4. turn the pretty ruby into ugly php. :P
