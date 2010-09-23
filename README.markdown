# Acts as tree for Rails 3

Specify this *acts_as* extension if you want to model a tree structure by providing a parent association and a children
association. This requires that you have a foreign key column, which by default is called *parent_id*.

## Generate a database migration

In terminal:
<code>rails g model Category name:string parent_id:integer</code>

Should generate a migration

<pre>
  class CreateCategories < ActiveRecord::Migration
    def self.up
      create_table :categories do |t|
        t.string  :name
        t.integer :parent_id

        t.timestamps
      end
    end

    def self.down
      drop_table :categories
    end
  end  
</pre>

## Migrate the database

In terminal:
<code>rake db:migrate</code>

Should create the database table called 'categories'.

## Add ActAsTree to the model

<pre>  # app/models/category.rb
  class Category < ActiveRecord::Base
    acts_as_tree :order => "name"
  end
</pre>

## Creaete a Seed file to seed the database with tree data (nodes)

<pre>  # db/seed.rb

  root      = Category.create("name" => "root")
  child1    = root.children.create("name" => "child1")
  subchild1 = child1.children.create("name" => "subchild1")
</pre>

## Seed the database!

In terminal:
<code>rake db:seed</code>

## API usage

<pre>
  Example:
  root
   \_ child1
        \_ subchild1
        \_ subchild2

  root      = Category.create("name" => "root")
  child1    = root.children.create("name" => "child1")
  subchild1 = child1.children.create("name" => "subchild1")

  root.parent   # => nil
  child1.parent # => root
  root.children # => [child1]
  root.children.first.children.first # => subchild1  
  
</pre>

## License

Copyright (c) 2007 David Heinemeier Hansson, released under the MIT license  

Includes patch from http://dev.rubyonrails.org/ticket/1924
