---
layout: page
title: "Rails 2.0 Command-line Shortcuts"
date: 2008-04-08 14:23
comments: true
sharing: true
footer: true
---
I just ran across a little confusion with Rails 2 command-line shortcuts.  By shortcuts, I mean things like this:

```
$ script/generate model pizza size:integer name:string description:text
      exists  app/models/
      exists  test/unit/
      exists  test/fixtures/
      create  app/models/pizza.rb
      create  test/unit/pizza_test.rb
      create  test/fixtures/pizzas.yml
      exists  db/migrate
      create  db/migrate/004_create_pizzas.rb
```

As you can see, it went ahead and created the model, test, fixture and migration.  The column definitions were stuffed auto-magically into the migration.

This is really cool, but it has some limitations.  If you are doing anything more complicated than it won't work for you.  For example, if we wanted to do a many-to-many relationship with a toppings table, you would have to set that up manually (as far as I know).

The confusion that I referred to earlier has to do with generating a migration.  Let's say that I already created my topping model manually by creating an `app/model/topping.rb` file.  Then it occurs to me that I also need a migration, so I run the following:

```
$ script/generate migration CreateTopping name:string price:float
```

Unfortunately, this will create the migration file, but the _column declarations are ignored_.  This is confusing, given the following behavior:

Let's say, instead, I created the toppings model with the following command:

```
$ script/generate model topping
```

This would have generated an empty `###_create_toppings.rb` migration.  Then I decide to add the columns to it like so:

```
$ script/generate migration AddNamePriceToToppings name:string price:float
```

Rails does some _magic_ and infers from the migration name that we are adding columns to the topping table.  But why didn't it add columns in the earlier example?  My only explanation is that migration generation is for updating already existing models/tables, it's not intended for model/table creation.  Arguably, what good is a created table without an associated model?  For simple cases, this holds true.  For a many-to-many situation, where you need to define a join table, this kind of shortcutting will only take you so far.

