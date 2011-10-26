---
layout: page
title: "Problem with Four Days on Rails"
date: 2008-04-03 16:40
comments: true
sharing: true
footer: true
---
I am working with a new developer who has Ruby experience, but no Rails experience.  I pointed him at the Rails site and told him to run though some of the tutorials.  I was surprised that the [Four Days on Rails](http://rails.homelinux.org/) tutorial had such a weird introduction to database stuff.  

When it has you create your database, it has you run the following sql code:

```
CREATE TABLE `categories` ( 
  `id` smallint(5) unsigned NOT NULL auto_increment, 
  `category` varchar(20) NOT NULL default '', 
  `created_on` timestamp(14) NOT NULL, 
  `updated_on` timestamp(14) NOT NULL, 
  PRIMARY KEY  (`id`), 
  UNIQUE KEY `category_key` (`category`) 
) TYPE=MyISAM COMMENT='List of categories'; 
```

What I don't get is: how is that any better/easier than giving a quick run through the migration process?  The above code could just as easily been replaced with:

After creating your database, and setting your `database.yml` connection variables, we will be building a table to hod our To Do list categories.  We will need a model definition for a Category, and using a generator, we get a place to define both the database table for the Categories and the model for a Category.

```
$ script/generate model category
      exists  app/models/
      exists  test/unit/
      exists  test/fixtures/
      create  app/models/category.rb
      create  test/unit/category_test.rb
      create  test/fixtures/categories.yml
      create  db/migrate
      create  db/migrate/001_create_categories.rb
```

Edit the migration so that it looks like this:

```
class CreateCategories < ActiveRecord::Migration
  def self.up
    create_table "categories", :force => true do |t|
      t.column :category,                     :string
      t.column :created_on,                :datetime
      t.column :updated_on,                :datetime
    end
  end

  def self.down
    drop_table "categories"
  end
end
```

Then run `rake db:migrate` and you have your table and you are now ready to edit the model.  

I don't see this as being worse than the SQL.  Why isn't it presented in this more "railsy" way?
