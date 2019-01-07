---
layout: post
title: Add a new database migration tips in Rails
categories: [Tech]
tags: [rails, database]
---

![](/images/Bing_714.JPG)

When you are using MySQL, if a migration fails the DDL will not be rolled back with the transaction, potentially leaving the database in an invalid state, which could theoretically bring the whole application down.

### Referencing model classes in a migration
Generally you would not need to reference model classes in a migration unless you were modifying existing data in some way, or adding new data. Sometimes we need to do this, but referencing application model classes raises some problems.

* If a model class is referenced in one migration, then the model's table is altered in a subsequent migration, the annotation written to the model's file will not reflect the subsequent alteration.
* Old migrations referencing the model might fail if the model code has evolved significantly since the time the migration was written.

To avoid these problems, you should:

1. Avoid using Active Record models in your migrations by using SQL
2. If you must use Active Record models, create a migration-specific class for the purpose.

Let's look at an example where we want to strip leading and trailing whitespace from a column. A naive implementation might look something like this:

```ruby
class MyMigration < ActiveRecord::Migration
  def change
    User.where.not(name: nil).each do |user|
      user.update name: user.name.strip
    end
  end
end
```

### Using SQL
Of course, doing this in SQL is quite simple and far more efficient than using Active Record to load and modify each record individually:

```ruby
class MyMigration < ActiveRecord::Migration
  def change
    execute <<-SQL.squish
      UPDATE users
         SET name = LTRIM(RTRIM(name))
       WHERE name IS NOT NULL
    SQL
  end
end
```
Note: that squish-ing your SQL is recommended because it makes the migration output a lot easier to read.

### Using a migration-specific model class
Some data migrations can't be easilly expressed in SQL, so let's say we want to stick with using Active Record. In such cases, we can create a migration specific class for the purpose:

```ruby
class MyMigration < ActiveRecord::Migration
  class User < ActiveRecord::Base
  end

  def change
    User.where.not(name: nil).each do |user|
      user.update name: user.name.strip
    end
  end
end
```
Here the migration will use the `MyMigration::User` class instead of `::User`
