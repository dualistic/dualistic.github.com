---
layout: post
title: "SOAPing With Ruby and PHP"
date: 2011-10-26 21:10
comments: true
categories: [soap, ruby, php, prototype]
---
So, there are some folks on campus who would like to have their faculty's journals listed on their faculty pages. This is certainly doable with some web-scraping, but the Web of Science (Web of Knowledge, actually) has an API, and the folks I have talked to have secured access to said API. 

Unfortunately, it is a SOAP API, something which I know nothing about. But, hey, how hard can it be?
<!--more--> 
My plan is: learn and prototype with ruby, and then fool around a little with PHP (to make sure it works), then make a bid to do the work (and include my already spent time :P ). 

### SOAPed up Ruby ###

My first stop for a library search is, of course, the excellent [Ruby Toolbox](http://www.ruby-toolbox.com).  They list [three options](https://www.ruby-toolbox.com/categories/soap): 

* [Savon](http://rubygems.org/gems/savon)
* [Handsoap](http://github.com/troelskn/handsoap)
* [Serviceproxy](http://github.com/jeremydurham/serviceproxy)

The searching also indicated that [SOAP4R](http://rubygems.org/gems/soap4r) was the original ruby SOAP library, but that it also is slow, old, and nobody likes it.  I decided to try first with Savon because it was first.

Ruby:

{% gist 1321660 wos.rb %}

PHP:

{% gist 1321660 wos.php %}

### Thoughts: ###

* As mentioned in the gist, I ended up not using Handsoap because the class API was hard to figure out.  Savon was way easier.  However, I still had an issue with passing the query params into the soap body.  I ultimately ended up just using a blob of manually crafted xml (a hash failed!). So, not perfect, but a fine proof of concept.
* The PHP is shorter than the Ruby!  Mostly this is because of the fact that I couldn't get a hash to work (and had to make raw xml).  But also it is because I made a class (in anticipation of using both Savon and Handsoap).  Even still, building the associative array in PHP drove me bonkers (seriously?  i have to use the array() function _every time_?)!
* Auth info is sent in the header.  This seems sensible.  The user/pass is base64 encoded, appended to "Basic ", and then set in an "Authorization" header.  Fine.  Annoyingly, the ruby `Base64.encode64()` method appends an \n to the encoded string so you have to `encode64(string).strip` or Mr. SOAP API will throw a 400. Anyway, our access is auth'ed via IP, so this doesn't matter and I never did get to see what a successful user/pass auth looked like.
* I have to pretend to build a cookie and send headers on my http requests as though I was holding a cookie, even though I am just pretending. The cookie games feel awkward.
* The ?wsdl business is pretty neat.  I like that the SOAP endpoints can tell you what api calls they respond to. I need to look at this more, can they tell you how to properly form a particular API call?  I mean, just knowing that you can do a :search doesn't mean you know the format of the query...
