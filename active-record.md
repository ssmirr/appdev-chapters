# ActiveRecord

`ActiveRecord` comprises a set of classes within Ruby on Rails that help us interact with our database tables, and is one of the most powerful reasons to choose Rails.

Assuming that we have a database and a table called "contacts" in it (we'll discuss how to do that in a moment), we would normally have to write Structured Query Language to interact with it.

Instead, we can define a class, e.g. `Contact`, and [inherit](https://chapters.firstdraft.com/chapters/769#inheritance){:target="_blank"} from `ApplicationRecord`:

```ruby
class Contact < ApplicationRecord
end
```

Then _boom_ — our class `Contact` has now inherited[^activerecord] a _tremendous_ number of powerful methods to interact with the `contacts` table, most of them related to **C**reating, **R**eading, **U**pdating, and **D**eleting records.

[^activerecord]: Actually, these methods mostly come from our grandparent class `ActiveRecord::Base` rather than our immediate parent class `ApplicationRecord`. We'll go into these details later.

We refer to these database-related classes as **models**, and we place them in the `app/models` folder within our Rails app. These classes talk to the database for us, contain most of our business logic, and are, in many ways, the heart of our applications.

## The quick way to create a table

Suppose we wanted to build an app called Rolodex that helps us keep track of our contacts. For this app, we need a table (let's call it "contacts") with three columns: first name, last name, and date of birth.

In Rails, anything that can be automated, _is_ automated. Much like we used the `rails new` command to generated dozens of folders and hundreds of files of boilerplate code, we can use another command to generate the boilerplate code that goes into creating a database table. Here is what the command looks like:

```bash
rails generate model contact first_name:string last_name:string date_of_birth:date
```

 - All of the generators, of which there are many, are invoked by starting with `rails generate`.
 - We select which generator we want with the third part: in this case, `draft:model`. "Model"
  is the word we use to refer to the Ruby classes that represent our database tables, because we use those tables to _model_ the entities in our problem domains.
 - The fourth part is the name we want for the class/table; singular (the Ruby class name will be singular, as we've seen with e.g. `Person`, even though we usually think about table names as plural).
 - After that, we provide a list of the columns we want in the table, along with each column's datatype.

We need to run this command at a command prompt. The Terminal window that is running the web server is stuck in an infinite loop of listening for web requests, so we can't run any commands in there. Open another Terminal window (from the Terminal menu, select New), paste in the command above at the `$` prompt, and press <kbd>return</kbd>.

You'll see some output in the Terminal that looks something like this:

```bash
invoke  active_record
create    db/migrate/20190422125330_create_contacts.rb
create    app/models/contact.rb
```

Next, run this command:

```bash
rails db:migrate
```

You'd see some output in the Terminal that looks something like this:

```bash
== 20190422125330 CreateContacts: migrating ===================================
-- create_table(:contacts)
   -> 0.0037s
== 20190422125330 CreateContacts: migrated (0.0037s) ==========================
```

That's it — with these two commands, you now have a fully-formed database table _and_ a Ruby class that will help us easily interact with it.

We could just as easily add another table to our database — maybe a table called "companies" with columns "name", "industry", and a few others. Run these two commands:

```
rails generate draft:model company name:string industry:string structure:string last_year_revenue:integer founded_on:date

rails db:migrate
```

Voilá — now we have two tables, and are ready to CRUD rows in them.

## Time to CRUD

### Command prompt vs rails console

Let's learn the methods we've inherited from `ActiveRecord` to make it easy. To do so, open a new Terminal and run the command `rails console` (or `rails c` for short). This will change the prompt from

```
$
```

to

```
[1] pry(main)>
```

The former is the **command prompt** and the latter is the **rails console**. _It's important to always know which one you are in._

`rails console` is an interactive way to try out Ruby with immediate feedback. It's quicker than writing into a file and then running it. You can only do Ruby in here (like `"hello".capitalize`). You _cannot_ do command prompt things (like `cd` or `ls` or `rails generate migration...`).

If you want to get out of rails console and back to the command prompt, which is like quitting an app to get back to the desktop of your computer, then type `exit` — or just open a new Terminal window.

## CREATE

### new

In rails console, try the following:

```ruby
Contact.all
```

You'll see that we get back an empty array. Right now, we have no rows in our table. We can confirm this with `Contact.all.count`. We do `Contact.all.count` so often that Rails lets us take a shortcut — we can just do `Contact.count` directly.

A crucial thing to remember: when you are talking to the **whole table**, you are referencing the _class_ `Contact`, so **use a capital letter**. If you did `contact.count`, what error message would you expect? Try it and see.

Like any Ruby class[^literal_shorthand] we instantiate a new, blank object with the `.new` method:

[^literal_shorthand]: Remember Array, Hash, or even String, before we got to their [literal](https://chapters.firstdraft.com/chapters/758#array-literals) [shorthand](https://chapters.firstdraft.com/chapters/767#hash-literals) [syntaxes](https://chapters.firstdraft.com/chapters/757#string-literals)? We always started with `.new`, and then built up from scratch.

```ruby
c = Contact.new
```

As usual we store the new object in a variable so that we can continue to work with it; in this case, we named the variable `c`. This object has attribute setter methods for every column in the table:

```ruby
c.first_name = "Raghu"
c.last_name = "Betina"
```

We didn't have to declare the `attr_accessor`s like we did for pure Ruby classes — by inheriting from `ApplicationRecord`, our `Contact` class gains superpowers. It connects to the database, figures out what columns are in the "contacts" table, and automatically defines methods to access each column that exists.

### save

So far, this is just like a pure Ruby class with `attr_accessor`s. But, crucially, we have the `.save` method now. Try it:

```ruby
c.save
```

Whoa! Some fancy new output. What you see is the actual SQL that is generated in order to transact with the database and save the data permanently to a row.

Now you can just type `c` and it should show you that `c` has been inserted into the database **and it has been assigned an ID number**.

If you exit `rails console`, shut down the computer, and come back into `rails console` tomorrow, and do `Contact.count`, you will see `1` — _the data will still be there_ (although the variable `c` will not — try it). We have saved it _permanently_. Try `Contact.all`, and you will see that the array of rows is no longer empty!

Add a few more contacts:

```ruby
c = Contact.new
c.first_name = "Minnie"
c.last_name = "Mouse"
c.date_of_birth = "November 18, 1928"
c.save
Contact.count # => 2

c = Contact.new
c.first_name = "Mickey"
c.last_name = "Mouse"
c.date_of_birth = "May 15, 1928"
c.save
Contact.count # => 3
```

Why does it work to re-use the variable `c` here? Well, when we `.save` on the prior one, we've entered the record in to the table and it is assigned an ID. Then we throw away the contents of the _variable_ `c` and replace it with a brand new, blank row with `Contact.new` — but that's okay, because the old data is stored on disk and we can always look it up by its ID number if we need it.

## READ

### all

**Returns:** an array of records

To retrieve all of the rows from a table, you can call `.all` on the class:

```ruby
contact_list = Contact.all
```

The return value of `.all` is an `Array`-like object (technically it's an `ActiveRecord::Relation`, but we can do all of our usual `Array` methods on it — `.each`, etc). From now on I'll refer to these `Array`-like objects as "**collections**".

### count

**Returns:** an `Integer`

We've already met `.count`, which tells you how many records are in a collection:

```ruby
Contact.all.count
```

### first

**Returns:** a single record

```ruby
c = Contact.all.first
```

### last

**Returns:** a single record

Returns the last record in a collection.

```ruby
c = Contact.all.last
```

### Attribute getter methods

However you got it, once you have an individual row stored in a variable, let's call it `c`, then you have a method for each column to retrieve the value for that cell:

```ruby
c = Contact.all.last

c.id
c.first_name
c.last_name
c.date_of_birth
c.created_at
c.updated_at
```

What is the return value if you try calling a method for a column that doesn't exist?

```ruby
c.middle_name
```

**RTEM!** You're going to see it _a lot_.

### order

**Returns:** an array of records

The `.order` method lets you sort your collections by one or more columns. The argument to `.order` is a `Hash`, where the _key_ is the _column_ you want to sort by, and the _value_ is either `:asc` (for ascending order) or `:desc` (for descending order):

```ruby
Contact.all.order({ :last_name => :asc })
```

If you send `.order` a `Symbol` alone, outside of a `Hash`, then ascending order is assumed:

```ruby
Contact.all.order(:last_name)
```

To break ties, you can provide multiple columns in the `Hash`:

```ruby
Contact.all.order({ :last_name => :asc, :first_name => :asc, :date_of_birth => :desc })
```

This would first order by last name, then break ties using first name, then break ties using date of birth.

### reverse

**Returns:** an array of records

`.reverse` reverses the ordering of a collection. Not particularly common, since you can use `:asc` and `:desc` to specify the direction you want explicitly, but there it is:

```ruby
Contact.order(:first_name).reverse
```

### where

**Returns:** an array of records

Perhaps the most important READ method is `.where`. This is our bread-and-butter tool for _filtering_ a collection of rows down using various criteria.

The argument to `.where` is a `Hash`, where the _key_ is the _column_ you want to filter by, and the _value_ is the criteria you want to filter by:

```ruby
Contact.all.where({ :id => 2 })
```

A bit of syntactic sugar — to save us some typing, we can call `.where` directly on the class if we want to, rather than calling `.all` first:

```ruby
Contact.where({ :last_name => "Mouse" })
```

You can provide multiple columns to filter by in the `Hash`:

```ruby
Contact.where({ :last_name => "Mouse", :first_name => "Minnie" })
```

### where always returns a collection, not a single row

**The return value from `.where` is always a collection, regardless of how many results there are.**

Whether there are 0, 1, or a million results, you still have an array. What would you expect if you tried the following?

```ruby
Contact.where({ :id => 2 }).first_name
```

Try it. RTEM!

 So, **use `.at(0)`** (a.k.a `.first`)  or `.at(-1)` (a.k.a. `.last`) or some other method **to retrieve an element** from the array if you want **to do something with an individual record**:

```ruby
c = Contact.where({ :id => 2 }).at(0)
c.first_name
```

### Using where with an array of criteria

**Returns:** an array of records

You can even use an `Array` in the argument to `.where`; it will then bring back the rows that match _any_ of the criteria for that column:

```ruby
Contact.where({ :last_name => ["Betina", "Woods"] })
```

### Chaining wheres

**Returns:** an array of records

Since `.where` returns another collection, you can chain `.where`s one after the other:

```ruby
Contact.where({ :last_name => "Mouse" }).where({ :first_name => "Minnie" })
```

This _narrows_ the search.

### where(this).or(that)

**Returns:** an array of records

You can _broaden_ the search with `.or`:

```ruby
Contact.where({ :first_name => "Mickey" }).or(Contact.where({ :last_name => "Betina" }))
```

This may look a little funny. We tack `.or` onto the end of one collection, and the argument to `.or` is an _entire query_, starting from the class again. The return value is both collections merged together.

### where.not(this)

**Returns:** an array of records

You can _negate_ a criteria with `.not`:

```ruby
Contact.where({ :last_name => "Mouse" }).where.not({ :first_name => "Mickey" })
```

You tack `where.not` on to a collection and it accepts all the same arguments as `.where`, but the result set is all of the records in the original collection _except_ the ones that match the criteria.

### Where is everything

**Everything from looking up a movie's director to putting together a feed in a social network ultimately boils down to `.where`s and `.each`s.** I can't emphasize the importance of `.where` enough. Ask lots of questions.

### map_relation_to_array

**Returns:** a regular Ruby array of scalar values (_not_ records, and _not_ an individual value)

Once you've retrieved the right subset of records, you can peel off the values in just one column with `.map_relation_to_array`:

```ruby
Contact.where({ :last_name => "Mouse" }).map_relation_to_array(:first_name) # => ["Minnie", "Mickey"]
```

The `.map_relation_to_array` method returns an `Array` of values. This can be handy in conjunction with the ability to pass `.where` an `Array` of criteria to filter by, e.g.:

```ruby
shawshank_id = 4
shawshank_roles = Role.where({ :movie_id => shawshank_id })
cast_ids = shawshank_roles.map_relation_to_array(:actor_id) # => [12, 94, 34]
shawshank_actors = Actor.where({ :id => cast_ids }) # => the collection of Shawshank's actors
```

 - Returns an `Array` of values in the column.
    - _Not_ a single value, even if there was only one record in the `ActiveRecord_Relation`.
    - _Not_ an `ActiveRecord_Relation`, so you can no longer use methods like `.where`, `.order`, etc. You can use `Array` methods like `.each`, `.at`, etc.
 - The argument to `.map_relation_to_array` must be a `Symbol` that matches the name of a column in the table.
 - You cannot call `.map_relation_to_array` on an individual ActiveRecord row. If you want the value in a column for an individual row, simply call the accessor method directly:

    ```ruby
    # for an array of records
    people.last_name # undefined method for array; bad
    people.map_relation_to_array(:last_name) # => ["Betina", "Woods"]; good

    # for an individual record
    person.last_name # => "Woods"; good
    person.map_relation_to_array(:last_name) # undefined method for Person; bad
    ```

### distinct

It is sometimes possible to retrieve a collection of records that contains duplicates, which we may not want (because it will e.g. throw off our count). To remove duplicates, you can use the `.distinct` method:

```ruby
# Imagine Eddie's ID is 42 in the actors table:
eddies_roles = Role.where({ :actor_id => 42 })
# But what if Eddie played multiple roles in the same film?
eddies_movie_ids = eddies_roles.map_relation_to_array(:movie_id)
# => [1, 3, 3, 12, 19] This array now has duplicate IDs in it.
# If we use it to do a lookup of movies, we'll get duplicate rows:
bad_eddies_movies = Movie.where({ :id => eddies_movie_ids }) 
# We can use .distinct to solve the problem:
good_eddies_movies = Movie.where({ :id => eddies_movie_ids }).distinct
```

## UPDATE

To update a row, first you need to locate it:

```ruby
c = Contact.where({ :id => 2 }).first
```

And then assign whatever new values you want to:

```ruby
c.first_name = "Minerva"
```

And then **don't forget to save the changes**:

```ruby
c.save
```

That's it.

## DELETE

To delete a row, first find it:

```ruby
c = Contact.where({ :id => 2 }).first
```

And then,

```ruby
c.destroy
```

Intense.

## Less commonly used queries

### Fuzzy criteria

`.where` can also be used to search for partial matches by passing a fragment of SQL in a `String`, rather than passing a `Hash`:

```ruby
Contact.where("last_name LIKE ?", "%bet%")
```

The `?` in the first argument is a placeholder where the second argument, `"%bet%"`, gets inserted[^sql_injection].

The `%` characters are wildcards, which match anything in that position.

So that query would find all rows that have the fragment "bet" anywhere within the `last_name` column.

[^sql_injection]: This is an advanced safety feature of Rails that prevents [SQL injection attacks](https://en.wikipedia.org/wiki/SQL_injection){:target="_blank"}.

### Less than or greater than

You can search for rows less than or greater than certain criteria:

```ruby
Contact.where("date_of_birth > ?", 30.years.ago.year)
Contact.where("last_name >= ? AND last_name <= ?", "A", "C")
```

Notice that you can have multiple placeholders `?` in the SQL fragment, and the subsequent arguments will be plugged in in order.

That last query, for a value within a range, can also be written with a `Hash` and a `Range`:

```ruby
Instructor.where({ :last_name => ("A".."C") })
```

This is particularly handy for searching for records within a particular range of times:

```ruby
start_date = 7.days.ago
end_date = Date.today
Contact.where({ :created_at => (start_date..end_date) })
```

With Ruby `Range`s, two dots means inclusive of the second value, and three dots means exclusive of the second value. E.g., `(1..4)` is 1, 2, 3, and 4; `(1...4)` is only 1, 2, and 3.

### limit

**Returns:** an array of records

You can limit the number of records in a collection with `.limit`:

```ruby
Contact.where({ :last_name => "Mouse" }).limit(10)
```

This will return no more than 10 records.

### offset

**Returns:** an array of records

You can skip some rows in the result set with `.offset`. This is useful for e.g. retrieving the second page of records, or choosing a random record:

```ruby
Contact.where({ :last_name => "Mouse" }).offset(10).limit(10)
```

### maximum

**Returns:** a single value (of whatever datatype the column is)

You can calculate the largest/latest value in a particular column within a collection with `.maximum`:

```ruby
Contact.where({ :last_name => "Mouse" }).maximum(:date_of_birth)
```

### minimum

**Returns:** a single value (of whatever datatype the column is)

You can calculate the smallest/oldest value in a particular column within a collection with `.minimum`:

```ruby
Contact.where({ :last_name => "Mouse" }).minimum(:date_of_birth)
```

### average

**Returns:** a single value (`Integer` or `Float`)

You can calculate the average value in a particular column within a collection with `.average`:

```ruby
Review.where({ :venue_id => 4 }).average(:rating)
```

## Adding or removing columns from your table

We have a few tools to make changes to our database once we have already run our migrations.

First and foremost, we can generate new migrations to add new tables and modify existing tables. To modify existing tables, the most common tools we use are `add_column` and `remove_column`.

Generate a new **migration** (not a whole **model** like above) at a Terminal prompt with, for example:

```bash
rails g migration AddTitleToInstructors
```

or

```bash
rails g migration RemoveLastNameFromInstructors
```

Try to pick a name for the migration that's descriptive of the change that you want make, like I did above.

Then, go into the new migration file and add instructions to make the change you want within the `change` method (or the `up` method, if that's what you find inside instead of `change`):

```bash
def change
  add_column :instructors, :title, :string
end
```

or

```bash
def change
  remove_column :instructors, :last_name
end
```

Then execute the migration with `rails db:migrate` at a Terminal prompt.

If your database gets into a weird state (usually caused by deleting old migration files), your ultimate last resort is

```bash
rails db:drop
```

This will destroy your entire database and all the data within it. Then, you can fix your migrations and re-run all from scratch with `rails db:migrate`.

Much more information about migrations can be found at the official Rails Guide:

> http://guides.rubyonrails.org/migrations.html

## Conclusion

Now you have the ability to **Create**, **Update**, and **Delete** records. And by creatively chaining `.where`, `.not`, and `.or`, you can (with ActiveRecord's help) write pretty much any SQL query you're ever going to need in order to **Read** anything you want; from a simple one-to-many lookup all the way through a complicated self-referential social network feed. It's just going to require a bit of practice.
