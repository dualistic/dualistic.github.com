---
layout: post
title: "Print out DataMapper Query"
date: 2012-03-02 15:23
comments: true
categories: 
---

Have you ever wanted to find out what query DataMapper will use to perform a particular query?  This ends up being useful for a couple of circumstances.  First, it's useful for debugging and checking to see if DM is doing what you think it is.  Second, if you want to remove the overhead of creating ruby objects (this can be very inefficient and slow), but you still want to use DM, then you can build up the query and instead execute it raw (returning structs) instead of running a standard #get, #find, #first, etc.  This is still database agnostic as far as I can tell.

This has come up a few times and I had to go dredge it up out of IRC logs whenever I forgot it, so I am going to capture it here for posterity.
<!-- more -->

So, how does one do this? In short, you have to call the private #select_statement method on the adapter. 

For example:

``` ruby DataMapper raw pre-executed query
q = Yogo::Project.all.query
# => #<DataMapper::Query @repository=:default @model=Yogo::Project @fields=[#<DataMapper::Property::UUID @model=Yogo::Project @name=:id>, #<DataMapper::Property::String @model=Yogo::Project @name=:name>, #<DataMapper::Property::Boolean @model=Yogo::Project @name=:private>] @links=[] @conditions=(deleted_at = nil) @order=[#<DataMapper::Query::Direction @target=#<DataMapper::Property::UUID @model=Yogo::Project @name=:id> @operator=:asc>] @limit=nil @offset=0 @reload=false @unique=false> 

DataMapper.repository.adapter.send(:select_statement,q)
# => ["SELECT \"id\", \"name\", \"private\" FROM \"yogo_projects\" WHERE \"deleted_at\" IS NULL ORDER BY \"id\"", []] 
```

Awesome!  

However, it is a bit more complicated in that you actually are given an array of values where array[0] is the query, and array[1] is conditional values.  There will be a '?' in array[0] for each of the conditionals.  

For example:

``` ruby Query with conditions
q = Yogo::Project.all(:name => "Test").query
# => #<DataMapper::Query @repository=:default @model=Yogo::Project @fields=[#<DataMapper::Property::UUID @model=Yogo::Project @name=:id>, #<DataMapper::Property::String @model=Yogo::Project @name=:name>, #<DataMapper::Property::Boolean @model=Yogo::Project @name=:private>] @links=[] @conditions=(deleted_at = nil AND name = "Test") @order=[#<DataMapper::Query::Direction @target=#<DataMapper::Property::UUID @model=Yogo::Project @name=:id> @operator=:asc>] @limit=nil @offset=0 @reload=false @unique=false> 

DataMapper.repository.adapter.send(:select_statement,q)
# => ["SELECT \"id\", \"name\", \"private\" FROM \"yogo_projects\" WHERE (\"deleted_at\" IS NULL AND \"name\" = ?) ORDER BY \"id\"", ["Test"]] 
```

This presents a problem!  We need to join array[0] with array[1] such that the query is reconstructed.  Thanks to a handy [StackExchange post](http://stackoverflow.com/questions/7021835/replace-string-with-array-content-in-ruby), it turns out that ruby is pretty good at this:

``` ruby Map an array into a string with delimiters
query = ["SELECT \"id\", \"name\", \"private\" FROM \"yogo_projects\" WHERE (\"deleted_at\" IS NULL AND \"name\" = ?) ORDER BY \"id\"", ["Test"]] 
# => ["SELECT \"id\", \"name\", \"private\" FROM \"yogo_projects\" WHERE (\"deleted_at\" IS NULL AND \"name\" = ?) ORDER BY \"id\"", ["Test"]] 

query[0].gsub("?").each_with_index{|v,i| v = "\"#{query[1][i]}\"" }
# => "SELECT \"id\", \"name\", \"private\" FROM \"yogo_projects\" WHERE (\"deleted_at\" IS NULL AND \"name\" = \"Test\") ORDER BY \"id\"" 
```

There, two handy things all at once. ;)
