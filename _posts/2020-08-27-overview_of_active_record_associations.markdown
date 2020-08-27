---
layout: post
title:      "Overview of Active Record Associations"
date:       2020-08-27 09:20:18 -0400
permalink:  overview_of_active_record_associations
---


This is an overview of Active Record Associations, after reading this post, you will be able to:

- Understand the types of Active Record associations.
- Declare associations between Active Record models.
- Use the methods added to your models by creating associations.

An association is a connection between two Active Record models that is used add features to your code. Rails supports six types of associations:

- `belongs_to`
- `has_one`
- `has_many`
- `has_many :through`
- `has_one :through`
- `has_and_belongs_to_many`

By declaring one of these associations, you instruct Rails to maintain information between the instances of your models. The following is an overview of each Rails association.
#### `belongs_to`

A `belongs_to` association sets up a one-to-one connection with another model, meaning that each instance of the declaring model "belongs to" one instance of the other model. For example, if your application includes authors and blog posts, and each post can be assigned to exactly one author, you'd declare the blog post model this way:

```ruby
class Post < ApplicationRecord
	belongs_to :author
end
```

*Note:  `belongs_to` associations must use the singular term (i.e. author). Using the pluralized form (i.e. authors) in the above example would cause an error because Rails automatically infers the class name from the association name. If the association name is wrongly pluralized, then the inferred class will be wrongly pluralized too.*

The corresponding migration would look like this:

```ruby
class CreatePosts < ActiveRecord::Migration
 def change
	 create_table :authors do |t|
		 t.string :name
		 t.timestamps
	 end

	 create_table :posts do |t|
      t.belongs_to :author
      t.datetime :published_at
      t.timestamps
    end
  end 
end
```

After declaring the `belongs_to` association, the declaring class automatically gains 6 methods related to the association:

- `association`
- `association=(associate)`
- `build_association(attributes = {})`
- `create_association(attributes = {})`
- `create_association!(attributes = {})`
- `reload_association`

In the above methods, `association` is replaced with the symbol passed as the first argument to `belongs_to`. In our example where posts belong to authors, each instance of the post model will have these methods available: 

`author`

`author=`

`build_author`

`create_author`

`create_author!`

`reload_author`

These additional methods make your code cleaner and easier. For example, you could add a new author while writing a post:

```ruby
new_author = @post.build_author(name: "Your Name")
new_author.save
```

*Note: If you use the `build_` option, you'll have to persist your new `author` with `#save` while the `create_` option will persist to the database for you.*
#### `has_one`

A `has_one` association also sets up a one-to-one connection with another model, but unlike `belongs_to` association, `has_one` association indicates that each instance of a model contains only one instance of another model. For example, each customer would have only one account and you would declare the customer model as follows:

```ruby
class Customer < ApplicationRecord
	has_one :account
end
```

The corresponding migration would look like this:

```ruby
class CreateCustomers < ActiveRecord::Migration
	def change
		create_table :customers do |t|
			t.string :name
			t.timestamps
		end

		create_table :accounts do |t|
      t.belongs_to :customer
      t.sting :account_number
      t.timestamps
    end
  end
end
```

After declaring `has_one` association, the declaring class automatically gains 6 methods related to the association:

- `association`
- `association=(associate)`
- `build_association(attributes = {})`
- `create_association(attributes = {})`
- `create_association!(attributes = {})`
- `reload_association`

As previously mentioned, in the above methods, `association` is replaced with the symbol passed as the first argument to `has_one`. In our example of customers and accounts, each instance of the customer model will have these methods available: 

`account`

`account=`

`build_account`

`create_account`

`create_account!`

`reload_account`
#### `has_many`

A `has_many` association indicates a one-to-many connection with another model and is found in models corresponding to a `belongs_to` association. `has_many` association indicates that each instance of the model has multiple instances of another model. Going back to the example of authors and blog posts, the author model would be declared as follows: 

```ruby
class Author < ApplicationRecord
	has_many :posts
end
```

*Note: The name of the other model (i.e. posts) is pluralized when declaring a `has_many` association.*

The corresponding migration would look like this:

```ruby
class CreateAuthors < ActiveRecord::Migration
	def change
		create_table :authors do |t|
			t.string :name
			t.timestamps
		end

		create_table :posts do |t|
      t.belongs_to :author
      t.datetime :published_at
      t.timestamps
    end
  end
end
```

After declaring `has_many` association, the declaring class automatically gains 17 methods related to the association:

- `collection`
- `collection<<(object, ...)`
- `collection.delete(object, ...)`
- `collection.destroy(object, ...)`
- `collection=(objects)`
- `collection_singular_ids`
- `collection_singular_ids=(ids)`
- `collection.clear`
- `collection.empty?`
- `collection.size`
- `collection.find(...)`
- `collection.where(...)`
- `collection.exists?(...)`
- `collection.build(attributes = {}, ...)`
- `collection.create(attributes = {})`
- `collection.create!(attributes = {})`
- `collection.reload`

In the above methods, `collection` is replaced with the symbol passed as the first argument to `has_many`, and `collection_singular` is replaced with the singularized version of that symbol. In our example of authors and posts, each instance of the author model will have these methods:

`posts`

`posts<<(object, ...)`

`posts.delete(object, ...)`

`posts.destroy(object, ...)`

`posts=(objects)`

`post_ids`

`post_ids=(ids)`

`posts.clear`

`posts.empty?`

`posts.size`

`posts.find(...)`

`posts.where(...)`

`posts.exists?(...)`

`posts.build(attributes = {}, ...)`

`posts.create(attributes = {})`

`posts.create!(attributes = {})`

`posts.reload`
#### `has_many :through`

A `has_many :through` association sets up a many-to-many connection with another model, indicating that the declaring model can be matched with multiple instances of another model through a third model. For example, in the case of a workout studio where students sign up for classes with various instructors, association declarations would look like this:

```ruby
class Instructor < ApplicationRecord
	has_many :workout_classes
	has_many :students, through: :workout_classes
end

class Workout_class < ApplicationRecord
	belongs_to :instructor
	belongs_to :student
end

class Student < ApplicationRecord
	has_many :workout_classes
	has_many :instructors, through: :workout_classes
end
```

Note how workout_class class serves as a connector between instructor class and student class in the above example. This is that third model that helps to set up a many-to-many connection between students and instructors. 

The corresponding migration would look like this:

```ruby
class CreateWorkoutClasses < ActiveRecord::Migration
  def change
    create_table :instructors do |t|
      t.string :name
      t.timestamps
    end
 
    create_table :students do |t|
      t.string :name
      t.timestamps
    end
 
    create_table :workout_classes do |t|
      t.belongs_to :instructor
      t.belongs_to :student
      t.timestamps
    end
  end
end
```

`has_many :through` association is very useful for setting up shortcuts through nested `has_many` associations. Continuing with the workout studio example, a workout studio has many instructors and an instructor has many workout classes. It's possible to get a list of all workout classes offered at the given studio: 

```ruby
class Workout_studio < ApplicationRecord
	has_many :instructors
	has_many :workout_classes, through: :instructors
end

class Instructor < ApplicationRecord
	belongs_to :workout_studio
	has_many :workout_classes
end

class Workout_class < ApplicationRecord
	belongs_to :instructor
end

```

With `through: :instructors`  in the above example, Rails will now understand:

```ruby
@workout_studio.workout_classes
```
#### `has_one :through`

A `has_one :through` association is used to set up a one-to-one connection with another model, such that the declaring model can be matched with only one instance of another model by proceeding through a third model. In the case of a customer and account example, each customer has only one account, and each account is associated with only one account history. The customer model would look like this:

```ruby
class Customer < ApplicationRecord
	has_one :account
	has_one :account_history, through: :account
end

class Account < ApplicationRecord
  belongs_to :customer
  has_one :account_history
end
 
class AccountHistory < ApplicationRecord
  belongs_to :account
end
```

The corresponding migration would look like this:

```ruby
class CreateAccountHistories < ActiveRecord::Migration
  def change
    create_table :customers do |t|
      t.string :name
      t.timestamps
    end
 
    create_table :accounts do |t|
      t.belongs_to :customer
      t.string :account_number
      t.timestamps
    end
 
    create_table :account_histories do |t|
      t.belongs_to :account
      t.integer :credit_rating
      t.timestamps
    end
  end
end
```
#### `has_and_belongs_to_many`

A `has_and_belongs_to_many` association creates a direct many-to-many connection with another model. In this case, no third or intervening model is needed. If your application includes assemblies and parts, with each assembly having many parts and each part appearing in many assemblies. The association declarations would look like this:

```ruby
class Assembly < ApplicationRecord
  has_and_belongs_to_many :parts
end
 
class Part < ApplicationRecord
  has_and_belongs_to_many :assemblies
end
```

The corresponding migration would look like this:

```ruby
class CreateAssembliesAndParts < ActiveRecord::Migration
  def change
    create_table :assemblies do |t|
      t.string :name
      t.timestamps
    end
 
    create_table :parts do |t|
      t.string :part_number
      t.timestamps
    end
 
    create_table :assemblies_parts, id: false do |t|
      t.belongs_to :assembly
      t.belongs_to :part
    end
  end
end
```

Note the assemblies_parts join table in the above example that allows the `has_and_belongs_to_many` relationship to work. 

After declaring a `has_and_belongs_to_many` association, the declaring class automatically gains 17 methods related to the association:

- `collection`
- `collection<<(object, ...)`
- `collection.delete(object, ...)`
- `collection.destroy(object, ...)`
- `collection=(objects)`
- `collection_singular_ids`
- `collection_singular_ids=(ids)`
- `collection.clear`
- `collection.empty?`
- `collection.size`
- `collection.find(...)`
- `collection.where(...)`
- `collection.exists?(...)`
- `collection.build(attributes = {})`
- `collection.create(attributes = {})`
- `collection.create!(attributes = {})`
- `collection.reload`

In the above methods, `collection` is replaced with the symbol passed as the first argument to `has_and_belongs_to_many`, and `collection_singular` is replaced with the singularized version of that symbol. In our example of assemblies and parts, each instance of the part model will have these methods:

`assemblies`

`assemblies<<(object, ...)`

`assemblies.delete(object, ...)`

`assemblies.destroy(object, ...)`

`assemblies=(objects)`

`assembly_ids`

`assembly_ids=(ids)`

`assemblies.clear`

`assemblies.empty?`

`assemblies.size`

`assemblies.find(...)`

`assemblies.where(...)`

`assemblies.exists?(...)`

`assemblies.build(attributes = {}, ...)`

`assemblies.create(attributes = {})`

`assemblies.create!(attributes = {})`

`assemblies.reload`

When deciding whether to use `has_many :through` or `has_and_belongs_to_many`, consider whether you need to have the independent relationship model. If you don't need to do anything with the relationship model (for example, have validations, callbacks or other attributes), it may be simpler to set up a `has_and_belongs_to_many` relationship and the corresponding join table. Otherwise, use `has_many :through`.

Active Record associations are versatile and powerful tools. As mentioned above, declaring associations adds many useful methods to your models that will help keep your code succinct. A much more detailed guide to associations can be found in the  in the [Rails Associations Guide](http://guides.rubyonrails.org/association_basics.html) and [Rails API docs](https://api.rubyonrails.org/classes/ActiveRecord/Associations/ClassMethods.html).
