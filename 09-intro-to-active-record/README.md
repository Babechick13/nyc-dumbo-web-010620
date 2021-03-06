Intro to ActiveRecord
===

## SWBATs
- [ ] Describe how gems work and the value of shared code
- [ ] Implement ActiveRecord in their projects
- [ ] Practice creating migrations for updating the database structure
- [ ] Explain how `rake` works and how to run rake tasks
- [ ] Distinguish between and define "model", "class", and "table"
- [ ] Practice with ActiveRecord::Base instance and class methods
- [ ] Perform persistent CRUD actions on one model using ActiveRecord

### Outline
* Demonstrate Mass Assignment
* Review project setup
* Show how to use `rake` tasks
* Choose a domain and set up ActiveRecord on our models (Game >-< Player)

0. Setup the environment

1. Create a migration (a set of instructions for maniulating the structure of the database)
  `rake db:create_migration NAME=create_games`

2. Write the migration
  ```rb
  create_table :games do |t|
    t.string :title
    t.string :genre
    t.integer :price

    t.timestamps
    # created_at, updated_at
  end
  ```

3. Run the migration `rake db:migrate`

3.5 Check the migration
   
4. Create model and connect to active record

  * Demonstrate how to work with migration files: `migrate` and `rollback`
  * Write CRUD on a model using ActiveRecord

### Metaprogramming and Mass Assignment
Is the concept of writing code that writes code. In ruby, everything is an object. We interact with objects by *sending messages* to them. Objects know which messages to respond to and how via the *methods* that we define in the class.

Therefore, when initializing an object in ruby, we can pass in a hash, and then iterate over the hash to Mass Assign attributes to the instance that we are creating.

```ruby
attr_accessor :attr1, :attr2, # etc...
def initialize(attribute_hash)
  attribute_hash.each do |key, value|
    self.send("#{key}=", value)
  end
end
```

### ActiveRecord

* What are the steps for setting up ActiveRecord on a model?

* How can we do CRUD actions using ActiveRecord?

### Rake

Rake lets us save some set instructions we want the computer to run into a command that we can call in the terminal using `rake` ie: `rake db:create_migration NAME="create_tweets"`

If we want to check what tasks we have available to us, we can do `rake --tasks` or `rake -T`

### Migrations
Migrations are ActiveRecord keeps track of database changes over time.

We specify how we want the database to change in the migration file: [docs](https://guides.rubyonrails.org/active_record_migrations.html)

* We can create migrations using `rake db:create_migration NAME="the_name_we_want`
* We can check what migrations have been run so far using `rake db:migrate:status`
* We tell ActiveRecord to apply what we wrote in the migration files to the database using `rake db:migrate`

### ActiveRecord Conventions
ActiveRecord relies heavily on naming conventions to do its metaprogramming (aka *magic*), and following those naming conventions is important if you want ActiveRecord to work as intended.

For ActiveRecord to connect between a model and a table, the model name should be *singular* and the table name should be *plural*. ActiveRecord comes with `.singularize` and `.pluralize` methods that you can call on strings to check what how to follow naming convention:

```rb
  "tweet".pluralize
  # => "tweets"

  "tweets".singularize
  # => "tweet"

  "person".pluralize
  # => people

  "human".pluralize
  # => humen (????)
```

### Methods from ActiveRecord

* Model.new - creates a new instance in local memory without persistence
* Model#save - inserts or updates instance in db
* Model.create - Model.new + Model#save
* Model.all - all instances
* Model.first - instance with the lowest ID in the db
* Model.last - instance with the highest ID in the db
* Model.find - find one instance by id
* Model.find_by(attribute: value) - can find one instance by attribute-value pair(s)
* Model.where(attribute: value) - can find many instances by attribute-value pair(s)
* Model#update - takes a hash and updates an instance in db
* Model#destroy - delete a row in the database
* Model.destroy_all - delete all rows in a table

### Resources
[ActiveRecord Basics](https://guides.rubyonrails.org/active_record_basics.html)
[ActiveRecord Migrations](https://guides.rubyonrails.org/active_record_migrations.html)
[ActiveRecord Querying](https://guides.rubyonrails.org/active_record_querying.html)