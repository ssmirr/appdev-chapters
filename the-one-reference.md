# The One Reference

This document should be used as a quick reference; read the full explanations for more depth.

## ActiveRecord_Relation (array of records)

### All Array methods

`ActiveRecord_Relation`s [inherit](https://chapters.firstdraft.com/chapters/769#inheritance) from `Array`, so all `Array` methods work:

- [`.each`](#looping-through-arrays)
- [`.at()` (and its aliases `[]`, `.first` for `.at(0)`, `.last` for `.at(-1)`)](#at)
- [`.count`](#count)
- [`.reverse`](#reverse-Array)
- [`.sample`](#sample)
- [`.shuffle`](#shuffle)

### .where

`ActiveRecord_Relation#where(Hash) ⇒ ActiveRecord_Relation`

Call `.where` on an `ActiveRecord_Relation` to filter the array of records based on some criteria.

`where` returns an [`ActiveRecord_Relation`](#activerecord_relation-array-of-records) based on the given `Hash`. 

-   `.where` returns an `ActiveRecord_Relation` **regardless of how many results there are**. In particular, even if there is only one result, _the return value will still be another `ActiveRecord_Relation`_.

-   The argument to `.where` is usually a `Hash`:

    -   Filtering by an exact match within a column:

        ```ruby
        { :id => 42 }
        ```

    -   Filter by multiple columns at once:

        ```ruby
        { :photo_id => 23, :user_id => 42 }
        ```

    -   Filter using an `Array` (results will match _any_ of the values in the `Array`):

        ```ruby
        { :owner_id => [4, 8, 15 ] }
        ```

    -   Filtering within a range of values:

        ```ruby
        { :dob => (1.year.ago..Date.today) }
        ```

        ```ruby
        { :last_name => ("A".."C") }
        ```

    -   If you _really_ want to, you can omit the curly brackets around the `Hash` for brevity, and Ruby will figure out what you mean. So, ultimately, you can write something like this:

        ```ruby
        Contact.where(:last_name => ("A".."C"))
        ```

          instead of this:

        ```ruby
        Contact.where({ :last_name => ("A".."C") })
        ```

Related Methods: `.where.not`, `.or`, `.limit`, `.order`

[Full explanation](https://chapters.firstdraft.com/chapters/770#where)

### where.not

`ActiveRecord_Relation#where.not(Hash) ⇒ ActiveRecord_Relation`

Call `.where.not` on an `ActiveRecord_Relation` to filter the array of records to _exclude_ records based on the given `Hash`. `.where.not` will return an `ActiveRecord_Relation`.

```ruby
Contact.where({ :last_name => "Mouse" }).where.not({ :first_name => "Mickey" })
```

-   Returns an [`ActiveRecord_Relation`](#activerecord_relation-array-of-records).
-   The acceptable arguments to `.not` are the same as to `.where`; see that method for a list.

Related methods: `.where`, `.order`, `.limit`

[Full explanation](https://chapters.firstdraft.com/chapters/770#wherenotthis)

### .or

`ActiveRecord_Relation#or(ActiveRecord_Relation) ⇒ ActiveRecord_Relation`

Call `.or` on an `ActiveRecord_Relation` to _combine_ the array of records with another array of records:

-   Returns an [`ActiveRecord_Relation`](#activerecord_relation-array-of-records).
-   The argument to `.or` must be another `ActiveRecord_Relation` _from the **same** table_.
-   _Broadens_ the result set.

```ruby
Contact.where({ :first_name => "Mickey" }).or(Contact.where({ :last_name => "Betina" }))
```

[Full explanation](https://chapters.firstdraft.com/chapters/770#wherethisorthat)

### .order

`ActiveRecord_Relation#order(Hash) ⇒ ActiveRecord_Relation`

or

`ActiveRecord_Relation.order(Symbol) ⇒ ActiveRecord_Relation`

Call `.order` on an `ActiveRecord_Relation` to sort the array based on one or more columns. This method returns an `ActiveRecord_Relation`.

```ruby
Contact.all.order({ :last_name => :asc, :first_name => :asc, :date_of_birth => :desc })
```

-   Returns an [`ActiveRecord_Relation`](#activerecord_relation-array-of-records).

-   The argument to `.order` is usually a `Hash`.

    -   The keys in the `Hash` must be [`Symbol`](#Interlude:-Symbol)s that match names of columns in the table. These are the columns that will be used for sorting. If there are multiple keys, the order in which they are provided will be used to break ties.
    -   The value associated to each key must be either `:asc` (for ascending order) or `:desc` (for descending order).

-   The argument to `.order` can also just be one `Symbol`, a column name.

```ruby
Contact.all.order(:last_name)
```

In that case, `:asc` order is assumed.

[Full explanation](https://chapters.firstdraft.com/chapters/770#order)

### .limit

`ActiveRecord_Relation#limit(Integer) ⇒ ActiveRecord_Relation`

Call `.limit` on an `ActiveRecord_Relation` to cap the number of records in the array.

-   Returns an [`ActiveRecord_Relation`](#activerecord_relation-array-of-records).
-   The argument to `.limit` must be an `Integer`.

```ruby
Contact.where({ :last_name => "Mouse" }).limit(10)
```

[Full explanation](https://chapters.firstdraft.com/chapters/770#limit)

### .offset

`ActiveRecord_Relation#offset(Integer) ⇒ ActiveRecord_Relation`

Call `.offset` on an `ActiveRecord_Relation` to discard the first few records in the array:

-   Returns an [`ActiveRecord_Relation`](#activerecord_relation-array-of-records).
-   The argument to `.offset` must be an `Integer`.

```ruby
Contact.where({ :last_name => "Mouse" }).offset(10).limit(10)
```

[Full explanation](https://chapters.firstdraft.com/chapters/770#offset)

### .pluck

`ActiveRecord_Relation#pluck(Symbol) ⇒ Array`

Call `.pluck` on an `ActiveRecord_Relation` to retrieve the values stored in _just one column_ of the records, and discard all other data.

-   `.pluck` returns a regular Ruby [`Array`](#array) of scalar values in the column.

    -   _Not_ a single value, even if there was only one record in the `ActiveRecord_Relation`.
    -   _Not_ an `ActiveRecord_Relation`, so you can no longer use methods like `.where`, `.order`, etc. You can use `Array` methods like `.sort`, `.sample`, etc.

-   The argument to `.pluck` must be a `Symbol` that matches the name of a column in the table.

-   You cannot call `.pluck` on an individual ActiveRecord row. If you want the value in a column for an individual row, simply call the accessor method directly:

```ruby
Contact.all.pluck(:last_name) # => ["Betina", "Mouse", "Woods"]
```

```ruby
# for an array of records
people.last_name # undefined method for array; bad
people.pluck(:last_name) # => ["Betina", "Woods"]; good
```

[Full explanation](https://chapters.firstdraft.com/chapters/770#pluck)

### .maximum

`ActiveRecord_Relation#maximum(Symbol) ⇒ Object`

Call `.maximum` on an `ActiveRecord_Relation` to grab and return the biggest value of a particular column.

```ruby
User.all.maximun(:age)
# => 102
```

-   Returns a value from an `ActiveRecord` column.

[Full explanation](https://chapters.firstdraft.com/chapters/770#maximum)

### .minimum

`ActiveRecord_Relation#minimum(Symbol) ⇒ Object`

Call `.minimum` on an `ActiveRecord_Relation` to grab and return the smallest value of a particular column.

```ruby
Photo.all.minimum(:caption) 
# => "... a mind needs books as a sword needs a whetstone, if it is to keep its edge."
```

-   Returns a value from an `ActiveRecord` column.

[Full explanation](https://chapters.firstdraft.com/chapters/770#minimum)

### .sum

`ActiveRecord_Relation#sum(Symbol) ⇒ Integer` or `⇒ Float` or `⇒ String`

Call `.sum` on an `ActiveRecord_Relation` to find the sum of the values in a single column:

```ruby
@poem.scores.sum(:points)
```

-   Returns an [`Integer`](#integer) or [`Float`](#integer) (or even a [`String`](#string)), depending on datatype of the column that was summed.
-   The argument to `.sum` must be a `Symbol` that matches the name of a column in the table.

[Full explanation](https://chapters.firstdraft.com/chapters/770#sum)

### .average

`ActiveRecord_Relation#average(Symbol) ⇒ Integer` or `⇒ Float`

Call `.average` on an `ActiveRecord_Relation` to find the mean of the values in a single column:

```ruby
@restaurant.reviews.average(:rating)
```

-   Returns an [`Integer`](#integer) or [`Float`](#float), depending on datatype of the column that was averaged.
-   The argument to `.average` must be a `Symbol` that matches the name of a column in the table.

[Full explanation](https://chapters.firstdraft.com/chapters/770#average)

## `ActiveRecord` (A single record)

### All column names

You get a method for every column in the table; for example, if you have retrieved an individual record from the `Contact` table, you can call `.first_name`, `.last_name`, etc, on it.

This means that every `ActiveRecord` object will have methods `.id`, `.created_at`, and `.updated_at`, since every table has those columns.

[Full explanation](https://chapters.firstdraft.com/chapters/770#attribute-getter-methods)

### Any other instance methods you define in the class

It's often helpful to define your own instance methods in the model file; for example, you might want to define a method `.full_name`:

```ruby
class Contact < ApplicationRecord
  def full_name
    return self.first_name + " " + self.last_name
  end
end
```

You would then be able to call `.full_name` anywhere in the application that you wind up with an individual `Contact` object.

[Full explanation](https://chapters.firstdraft.com/chapters/769#defining-instance-methods)

## Association Helper Methods

### belongs_to

`belongs_to(Symbol, Hash) ⇒ ActiveRecord`

Let's say I have a movie in a variable `m`. It is annoying and error prone to, whenever I want the director associated with a movie, have to type

```ruby
d = Director.where({ :id => m.director_id }).at(0)
```

Wouldn't it be great if I could just type

```ruby
d = m.director
```

and it would know how to go look up the corresponding row in the directors table based on the movie's `director_id`?

Unfortunately, I can't, because `.director` isn't a method that `Movie` objects automatically know how to perform — the method would be undefined. (`Movie` objects know how to perform `.director_id` because we get a method for every column in the table.)

We could define such an instance method in the model ourselves without too much trouble. But, fortunately, since domain modeling and associations are at the heart of every application's power, Rails gives us a shortcut. Go into the `Movie` model and add a line like this:

```ruby
belongs_to(:director, { :class_name => "Director", :foreign_key => "director_id" })
```

This line tells Rails:

-   `belongs_to`: We only one result (not an array of results).
-   `:director`: Define a method called `.director` for all movie objects.
-   `:class_name => "Director"`: When someone invokes `.director` on a movie, go fetch a result from the directors table.
-   `:foreign_key => "director_id"`: Use the value in the `director_id` column of the movie to query the directors table for a row.

This is exactly what we would do if we defined the instance method by hand:

```ruby
def director
  return Director.where({ :id => m.director_id }).at(0)
end
```

Either way, we now can utilize this handy shortcut anywhere in our application when we have a movie `m` and we want to get the record in the directors table associated with it:

```ruby
m.director
```

Even better, if you've named your method and foreign key column conventionally (exactly matching the name of the other table), you can use the super-shorthand version:

```ruby
belongs_to(:director)
```

-   If you omit specifying the `:class_name`, Rails assumes that the table that you want to query is named the same thing as the method you are defining.

-   If you omit specifying the `:foreign_key`, Rails assumes that the foreign key column is named the same thing as the method plus `_id`.

-   If either of those things happens to not be true, then just include the `Hash` as the second argument to `belongs_to` and spell it all out:

    ```ruby
    belongs_to(:owner, { :class_name => "User", :foreign_key => "poster_id" })
    ```

    This would give us a method called `.owner` that returns a `User`, even though the foreign key column is called `poster_id`. We still have complete control, if we need to depart from conventional method/foreign key names for some reason.

### has_many

`has_many(Symbol, Hash) ⇒ ActiveRecord_Relation`

Let's say I have a director in a variable `d`. It is annoying and error prone to, whenever I want the films associated with the director, have to type

```ruby
f = Movie.where({ :director_id => d.id })
```

Wouldn't it be great if I could just type

```ruby
f = d.movies
```

and it would know how to go look up the corresponding rows in the movies table based on the director's `id`?

Unfortunately, I can't, because `.movies` isn't a method that `Director` objects automatically know how to perform — the method would be undefined.

We could define such an instance method in the model ourselves without too much trouble. But, fortunately, since domain modeling and associations are at the heart of every application's power, Rails gives us a shortcut. Go into the `Director` model and add a line like this:

```ruby
has_many(:movies, { :class_name => "Movie", :foreign_key => "director_id" })
```

This line tells Rails:

-   `has_many`: We want many results in an array.
-   `:movies`: Define a method called `.movies` for all director objects.
-   `:class_name => "Movie"`: When someone invokes `.movies` on a director, go fetch results from the movies table.
-   `:foreign_key => "director_id"`: Use the `director_id` column of the movies table to filter using the `id` of the director.

This is exactly what we would do if we defined the instance method by hand:

```ruby
def movies
  return Movie.where({ :director_id => self.id })
end
```

Either way, we now can utilize this handy shortcut anywhere in our application when we have a director `d` and we want to get the records in the movies table associated with it:

```ruby
d.movies
```

Even better, if you've named your method and foreign key column conventionally (exactly matching the name of the other table), you can use the super-shorthand version:

```ruby
has_many(:movies)
```

-   If you omit specifying the `:class_name`, Rails assumes that the table that you want to query is named the same thing as the method you are defining.

-   If you omit specifying the `:foreign_key`, Rails assumes that the foreign key column is named the same thing as this model plus `_id`.

-   If either of those things happens to not be true, then just include the `Hash` as the second argument to `belongs_to` and spell it all out:

    ```ruby
    has_many(:filmography, { :class_name => "Movie", :foreign_key => "director_id" })
    ```

    This would give us a method called `.filmography` that returns `Movie`s. We still have complete control, if we need to depart from conventional method/foreign key names for some reason.

### has_many/through

`has_many(Symbol, Hash) ⇒ ActiveRecord_Relation`

After you have established all of your one-to-many association helper methods, you can also add many-to-many helper methods with the `:through` option on `has_many`:

```ruby
class Movie < ApplicationRecord
   has_many(:characters)
   has_many(:actors, { :through => :characters, :source => :actor })
end

class Character < ApplicationRecord
  belongs_to(:movie)
  belongs_to(:actor)
end

class Actor < ApplicationRecord
   has_many(:characters)
   has_many(:movies, { :through => :characters, :source => :movie })
end
```

## String

### .concat (.+)

`String#concat(Integer) ⇒ String`

or 

`String#concat(String) ⇒ String`

Appends the given arguments to a string. when given an integer as an argument, it converts the integer into ASCII code.  

```ruby
"hi".concat(33) # => "hi!"
```

```ruby
"Rub".concat(121)  # => "Ruby"
```

When given a string literal as an argument, it adds that string to the original string. `.+` or `+` is _similar_ to the `.concat` method. `+` will return a new combined `String` instead of modifying the original `String`. Each line of code below will give the same output.  

`"hi".concat(" there")`  
`"hi".+(" there")`  
`"hi" +(" there")`  
`"hi" + " there"`  

This method returns a new [`String`](#string)

[Full explanation](https://chapters.firstdraft.com/chapters/757#string-addition-aka-)

### \* method

Multiplies the original string by the given integer and returns the new modified [`String`](#string).  

```ruby
"Ya" * 5
```

Returns `"YaYaYaYaYa"`

```ruby
"Ya" * 0
```

Returns `""`

[Full explanation](https://chapters.firstdraft.com/chapters/757#string-multiplication-aka-)

### .upcase

`String#upcase ⇒ String`

Converts all lowercase letters to their uppercase counterparts in the given string and returns the new modified [`String`](#string).  

```ruby
"hello".upcase`
```

Returns

`"HELLO"`

[Full explanation](https://chapters.firstdraft.com/chapters/757#upcase)

### .downcase

`String#downcase ⇒ String`

Converts all the uppercase letters to their lowercase counterparts from the given `String`. Returns the new modified [`String`](#string). 

```ruby
"HI".downcase`
```

Returns

`"hi"`

[Full explanation](https://chapters.firstdraft.com/chapters/757#downcase)

### .swapcase

`String#swapcase ⇒ String`

Converts all the uppercase letters to their lowercase counterparts and lowercase letters to their uppercase counterparts from the given string. Returns the new modified [`String`](#string).  

```ruby
"Hi There".swapcase
```

Returns

`"hI tHERE"`

* * *

```ruby
"hI tHERE".swapcase
```

Returns

`"Hi There"`

[Full explanation](https://chapters.firstdraft.com/chapters/757#swapcase)

### .reverse (String)

`String#reverse ⇒ String`

Returns a new [`String`](#string) with the characters from the original String in reverse order.  

```ruby
"stressed".reverse
```

Returns

`"desserts"`  

[Full explanation](https://chapters.firstdraft.com/chapters/757#reversed)

### .length

`String#length ⇒ Integer`

Returns the [`Integer`](#integer) number of charactersin the `String`.  

```ruby
"hippopotamus".length
```

Returns

`12`

[Full explanation](https://chapters.firstdraft.com/chapters/757#length)

### .chomp

`String#chomp ⇒ String`

or

 `String#chomp(String) ⇒ String`

When not given any argument, removes the `"\n"` (newline) character from the end of the string. When given an argument of a charcter or a string, it remove that argument from the _end_ of the orginal string. 

Returns a new [`String`](#string) with the character removed. 

```ruby
"Hey!\n".chomp  
"Hey!".chomp("!")  
"Hey There".chomp("There")  
"Hey!".chomp("y")
```

```ruby
"Hey!"  
"Hey"  
"Hey "  
"Hey!"
```

```ruby
p "What is your name?"
name = gets # supposes the user inputs "Clark" and then hits return
p "Hi" + name
```

Prints

`"Hi Clark\n"` 

```ruby
p "What is your name?"
name = gets # supposes the user inputs "Clark" and then hits return
p "Hi" + name.chomp
```

Prints

`"Hi Clark"`

```ruby
p "What is your name?"
name = gets.chomp # supposes the user inputs "Clark" and then hits return
p "Hi" + name
```

`"Hi Clark"`

[Full explanation](https://chapters.firstdraft.com/chapters/757#chomp)

### .gsub

`String#gsub(String, String) ⇒ String`

Substitutes the all occurances of the first argument with the second argument in original string and returns a new [`String`](#string) with the substitutes made.  

```ruby
"Hello".gsub("ello", "i")
```

Returns

`"Hi"`  

```ruby
"Hi.there".gsub(".", " ")
```

Returns

`"Hi there"`  

Giving an empty string as the second arugment deletes any occurences of the first arugument in the string.   

```ruby
"example @ ruby.com".gsub(" ", "")
```

Returns

`"example@ruby.com"` 

[Full explanation](https://chapters.firstdraft.com/chapters/757#gsub)

### .to_i (String)

`String#to_i  ⇒ Integer`

Converts a string literal that contains a number to an integer. The [`Integer`](#integer) is returned.  

```ruby
"8".to_i
```

Returns

`8`

```ruby
p "What is your lucky number?"
lucky_number = gets.chomp # Suppose the user types is "7" and then hits return
square = lucky_number ** 2 # This will throw an error.
```

Returns

`NoMethodError (undefined method '**' for "7":String)`

This is where the `.to_s` method comes handy.

```ruby
p "What is your lucky number?"
lucky_number = gets.chomp # Suppose the user types is "7" and then hits return
square = lucky_numbe.to_i ** 2 # This will throw an error.
p square
```

Returns

`"49"`

[Full explanation](https://chapters.firstdraft.com/chapters/757#to_i)

### .strip

`String#strip  ⇒ String`

Removes all leading and trailing whitespace in the string.  

Returns a new [`String`](#string) that has been modified from the original. 

```ruby
"   hi there ".strip
```

Returns

`"hi there"`

[Full explanation](https://chapters.firstdraft.com/chapters/757#strip)

### .capitalize

`String#capitalize ⇒ String`

Capitalizes the first character of a string.

Returns a new [`String`](#string) that has been modified from the original.

```ruby
"capitalize".capitalize
```

Returns

`"Capitalize"`

[Full explanation](https://chapters.firstdraft.com/chapters/757#capitalize)

### .split

`String#split ⇒ Array`

or

`String#split(String) ⇒ Array`

Splits a string into an substrings and creates an Array of these substrings. when not given argument, `.split` uses whitespace to divide the string. when given an argument, `.split` divides the string on that argument.

Returns an [`Array`](#array) of the divided [`String`](#string).

```ruby
"Hello hi byebye".split
```

Returns

`["Hello", "hi", "byebye"]`

```ruby
"one!two!three!".split("!")
```

Returns

`["one", "two", "three"]`

[Full explanation](https://chapters.firstdraft.com/chapters/757#split)

## Integer

whole numbers

### Math Operations for the Integer Class

`12 + 5 # => 17`  
`12 - 5 # => 7`  
`12 * 5 # => 60`  
`12 / 5 # => 2`  
The `/` operator for integers only returns a whole number and omits the remainder.

[Full explanation](https://chapters.firstdraft.com/chapters/760#-------math)

### `%` (modulus) operator

`Integer % Integer ⇒ Integer`

Returns the [`Integer`](#integer) remainder from a divisions.  

```ruby
13 / 5
```

Returns

`3`

### `**` operator Integer

`Integer ** Integer ⇒ Integer`

Raises a number to a power.  

Returns an [`Integer`](#integer)

`3 ** 2 # => 9`  
`2 ** 3 # => 8`

[Full explanation](https://chapters.firstdraft.com/chapters/760#-------math)

### .odd? and .even? method

`Integer#odd? ⇒ Boolean`

or

`Integer#even? ⇒ Boolean`

Returns a [_boolean_](#conditionals)(`true` or `false`) based on whether the integer is odd or even.

`7.odd?`

Returns

`true`

`8.odd?`

Returns

`false`

`8.even?`

Returns

`true`

`7.even?`

Returns

`false`

[Full explanation](https://chapters.firstdraft.com/chapters/760#odd-and-even)

### rand

`Integer#rand ⇒ Float`

or

`Integer#rand(Integer) ⇒ Integer`

or

`Integer#rand(Range) ⇒ Integer`

-   Creates a random [`Float`](#float) between 0 to 1
-   Can be given an optional `Integer` argument that will generate and return an [`Integer`](#integer) between 0 and the argument.
-   Can be given an optional argument of a `Range` that will generate a random [`Integer`](#integer) that between the `Range`.

```ruby
rand           # returns => 0.21374618638...
rand(10)       # returns => 7
rand((10..20)) # returns => 19
```

[Full explanation](https://chapters.firstdraft.com/chapters/760#rand)

### .to_s

`Integer#to_s ⇒ String`

Converts an integer to a string literal.

Returns a [`String`](#string)

`8.to_s`

> "8"

```ruby
lucky_number = rand(10) # assigns a random integer between 0 to 9 to the variable lucky_number
p "My lucky number is " + lucky_number + "!" 
```

The above block of code won't work and will throw an error.

`TypeError (no implicit conversion of Integer into String)`

This is where the `.to_s` method comes handy. 

```ruby
lucky_number = rand(10) # assigns a random integer between 0 to 9 to the variable lucky_number
p "My lucky number is " + lucky_number.to_s + "!" 
```

> "My lucky number is 7!"

`"There are " + 7.to_s + " pineapples."`

> "There are 7 pineapples"

[Full explanation](https://chapters.firstdraft.com/chapters/760#to_s)

### .to_f

`Integer#to_f ⇒ Float`

converts an integer to a [`Float`](#float)(decimal).  
`7.to_f`

> 7.0

```ruby
number = 10       #  
p number / 3      # Returns => 3
p number.to_f / 3 # Returns => 3.3333333333333335
```

[More examples](#integer-and-float-division-examples)

[Full explanation](https://chapters.firstdraft.com/chapters/760#to_f)

## Float

decimals

### Math Operations for the Float Class

Standard operations are similar to those for the Integer class. The only exception is the `/` operator which returns fractional results.

```ruby
12 / 5     # => 2 
12.0 / 5.0 # => 2.4 
12 / 5.0   # => 2.4  
12.0 / 5   # => 2.` 
```

### `**` Float operator

`Float ** Float ⇒ Float`

The `**`operator for Floats can additionally be used to calculate roots. 

```ruby
9 ** 0.5     # => 3.0, since 9^(1/2) = sqaureroot of 9  
8 ** (1/3.0) # => 2.0, since 8^(1/3) = cuberoot of 8  
```

[Full explanation](https://chapters.firstdraft.com/chapters/759#------math)

### .round

`Float#round ⇒ Integer`

or

`Float#round(Integer) ⇒ Float`

-   Returns the whole part ([`Integer`](#integer)) of a decimal when not given any argument.  
-   When given an argument, returns a [`Float`](#float) rounded to the number of decimal places specified by the argument.  

`3.14159.round # => 3`  
`3.14159.round(3) # => 3.142`  
`3.14139.round(3) # => 3.141`  

[Full explanation](https://chapters.firstdraft.com/chapters/759#round)

### .to_i (Float)

`Float#to_i  ⇒ Integer`

Converts a float to an integer by rounding the float down to closest whole number.

Returns an [`Integer`](#integer)  

`"8.9".to_i`  

> 8

`"8.1".to_i`  

> 8

#### Integer and Float division examples

`12 / 5 # => 2`  
`(12 / 5).to_f # => 2.0`  
`12.to_f / 5 # => 2.4`  
`12 / 5.to_f # => 2.4`  
`(12.0 / 5 ).to_i # => 2`  
`(12 / 5.0).to_i # => 2`  

## Date

### Creating a Date

To use the Date class in a Ruby program, we need to say:  
`require "date"`  
**_Note:_**
Only _Ruby_ programs need to have a `require` statement for the Date class. _Rails_ already does this for you.

### Date.new

`Date.new ⇒ Date`

Use the `.new` method to create a new instance of a Date object. The `.new` method can be used with or without argument. When given no arguments, the default date is set to _Jan 1st, -4712 BCE_.

```ruby
Date.new                  # => #<Date: -4712-01-01 ((0j,0s,0n),+0s,2299161j)>
Date.new(2001)            # => #<Date: 2001-01-01 ...>
Date.new(2001,2,3)        # => #<Date: 2001-02-03 ...>
Date.new(2001,2,-1)       # => #<Date: 2001-02-28 ...>
```

[Full explanation](https://chapters.firstdraft.com/chapters/768#datenew)

### Date.today

`Date.today ⇒ Date`

Initializes a Date object to the current date.

Returns a [`Date`](#date)

`Date.today # => #<Date: 2019-04-16 ((2458590j,0s,0n),+0s,2299161j)>`  

[Full explanation](https://chapters.firstdraft.com/chapters/768#datetoday)

### Date.parse()

`Date.parse(String) ⇒ Date`

Returns a [`Date`](#date) object initialized to a date, interpreted from the given String argument.

```ruby
Date.parse("2001-02-03")   # => #<Date: 2001-02-03 ...>
Date.parse("20010203")     # => #<Date: 2001-02-03 ...>
Date.parse("3rd Feb 2001") # => #<Date: 2001-02-03 ...>
```

[Full explanation](https://chapters.firstdraft.com/chapters/768#dateparse)

### Subtraction

Two dates can be subtracted from one another. The `-` operator returns a `Rational` which can be converted into an [`Integer`](#integer) to find the days in between the two dates.  

```ruby
number_of_days = Date.today - Date.parse("July 4, 1776") 
# => number_of_days = (88674/1)
number_of_days.to_i # => 88674
```

[Full explanation](https://chapters.firstdraft.com/chapters/768#subtraction)

### Date.mday

`Date.mday ⇒ Integer`

Returns the day of the month (1-31).

```ruby
held_on = Date.new(2001,2,3)
held_on.mday # => 3
```

[Full explanation](https://chapters.firstdraft.com/chapters/768#day)

### Date.wday

`Date.wday ⇒ Integer`

Returns the day of the week as an [`Integer`](#integer) (0-6, Sunday is 0).

```ruby
held_on = Date.new(2001,2,3)
held_on.wday # => 6
```

[Full explanation](https://chapters.firstdraft.com/chapters/768#wday)

### Days of the Week

`Date#moday? ⇒ Boolean`

```ruby
date = Date.new
date.monday?    # => true if date is a Monday.
date.tuesday?   # => true if date is a Tuesday.
date.wednesday? # => true if date is a Wednesday.
date.thursday?  # => true if date is a Thursday.
date.friday?    # => true if date is a Friday.
date.saturday?  # => true if date is a Saturday.
date.sunday?    # => true if date is a Sunday.
```

Returns a [`Boolean`](#boolean), `true` or `false`, if this given `Date` is a particular day of the week.

[Full explanation](https://chapters.firstdraft.com/chapters/768#monday)

## Array

list of objects represented with square brackets, \[].

### Creating an Array

`Array.new ⇒ Array`

initializes a new empty Array.  

`cities = Array.new # => cities = []`  
or  
`cities = [] # => cities = []` 

### .push

`Array#push(Object) ⇒ Array`

Adds elements to the end of an Array. Returns the modified [`Array`](#array).

```ruby
cities.push("Chicago")
cities.push("Los Angeles")
cities.push("New York City")
```

or  

```ruby
cities = ["Chicago", "Los Angeles", "New York City"]
# Initializes and adds elements to an Array
```

### .at()

`Array#.at(Integer) ⇒ Object`

Takes an Integer argument and return the element in that position of an Array. The following lines of code show the various forms of the `.at` method and return the same output.

Returns an `Object`  

```ruby
cities = ["Chicago", "Los Angeles", "New York City"]
cities.at(2)  
cities.[](2)  
cities[2]
```

> `"New York City"`

**_Note:_**
1. Ruby indexes the elements in an array starting at _zero_, that is, the first element of an array will have the index _zero_.
2. Trying to access an element using an index greater than the length of the array will give you `nil`.  
`cities.at(3) # => nil`
3. Using a negative index will retrieve elements from the end of the least.  
`cities.at(-1) # => "New York City"`  
`cities.at(-2) # => "Los Angeles"`  
`cities.at(-3) # => "Chicago"`  
`cities.at(-4) # => nil` 

[Full explanation](https://chapters.firstdraft.com/chapters/758#at)

### .first and .last

`Array#first ⇒ Object` or `Array#last ⇒ Object`

Retrieves and returns the first or the last element of an array.

Returns an `Object`

`cities.first # => "Chicago"`  
`cities.last) # => "New York City"`

### .index

`Array#index(Object) ⇒ Integer`

Returns an [`Integer`](#integer) that is the index of an element.  
`cities.index("Los Angeles") # => 1`

### .count

`Array#count ⇒ Integer` or `Array#count(Object) ⇒ Integer`

Returns the number of elements in a list, when give no arguments. If given an argument, returns the number of times that arguments occurs in the array. In both instances, this method returns an [`Integer`](#integer)

```ruby
nums = [8, 3, 1, 19, 23, 3]
nums.count # => 6
nums.count(3) # => 2
nums.count(2) # => 0
```

[Full explanation](https://chapters.firstdraft.com/chapters/758#count)

### .reverse (Array)

`Array#reverse ⇒ Array`

Returns a new [`Array`](#array)Array with the elements of the original Array but in the reversed order.  
`nums.reverse # => [3, 23, 19, 1, 3, 8]`

[Full explanation](https://chapters.firstdraft.com/chapters/758#reverse)

### .sort

`Array#.sort ⇒ Array`

Returns a new [`Array`](#array) with the elements of the original Array but in the sorted in increasing order.  
`nums.sort # => [1, 3, 3, 8, 19, 23]`

#### Example: Sorting an Array in decreasing order
```ruby
nums = [8, 3, 1, 19, 23, 3]
nums.sort # => [1, 3, 3, 8, 19, 23] 
nums.reverse # => [3, 23, 19, 1, 3, 8] 
nums.sort.reverse # => [23, 19, 8, 3, 3, 1], first sorts then reverses the Array. 
```
### .shuffle

`Array#shuffle ⇒ Array`

Returns a new [`Array`](#array) with the elements of the original Array but with the order shuffled randomly.  
`nums.shuffle # => [3, 23, 8, 19, 1, 3]`  
`nums.shuffle # => [19, 3, 1, 8, 3, 23]` 

[Full explanation](https://chapters.firstdraft.com/chapters/758#shuffle)

### .sample

`Array#sample ⇒ Array`

Returns a random element from the array.  
`nums.sample # => 23`  
`nums.sample # => 3`

[Full explanation](https://chapters.firstdraft.com/chapters/758#sample)

### .min and .max

`Array#min ⇒ Object` or `Array#max ⇒ Object`

Retrieve the elements of minimum and the maximum values in the array.  
`nums.min # => 1`  
`nums.max # => 23`

[Full explanation](https://chapters.firstdraft.com/chapters/758#min)

### .sum (Array)

`Array#sum`

Returns the sum of all the elements in the array.  
`nums.sum # => 57`

**Note** This method only works in the elements in the `Array` are _not_ a [`Hash`](#hash)

[Full explanation](https://chapters.firstdraft.com/chapters/758#sum)

## Hash

list of objects represented with curly brackets, {}. Unlike Arrays, each cell is not automatically numbered but given a label by us.

### Interlude: Symbol

Symbols are a sequence of characters and are used to to label something internally in the code. They are created by starting them off with a colon and follow the same naming conventions as variables, `:hello`.  
`:hello.class # => Symbol`

[Full explanation](https://chapters.firstdraft.com/chapters/767#a-brief-interlude-symbols)

### Creating a Hash

`Hash.new ⇒ Hash`

`person1 = Hash.new`  
or  
`person2 = {}`

### .store

`Hash#store(Object, Object) ⇒ Object`

Adds elements to a Hash by taking two arguments, a label (or _key_) and a piece of data (or _value_).

This method returns the `Object` that was stored.

The `key` can be any type, although is usually a `Symbol`. The value can also be of any type.

```ruby
person1.store(:first_name, "Raghu")
person1.store(:last_name, "Betina")
person1.store(:role, "Instructor")
# => person1 = {:first_name=>"Raghu", :last_name=>"Betina", :role=>"Instructor"}
```

or we can fill up a hash by typing in the hash literal  
`person2 = { :first_name => "Jocelyn", :last_name => "Williams", :role => "Student" }`

**_Note:_**
1. Ruby represents each key/value pair by separating them with a `=>`, known as a "hash rocket."
2. If the value associated with a key already exists when you try to `.store` something under it, its value will be replaced.

[Full explanation](https://chapters.firstdraft.com/chapters/767#creating-hashes)

### .fetch

`Hash#fetch(Object) ⇒ Object`

or

`Hash#fetch(Object, Object) ⇒ Object`

Both `.fetch` and `.[]` can be used to retrieves the data held by a key.
`person1.fetch(:last_name)# => "Betina"`  
`person2.[:last_name] # => "Williams"` 

If `.fetch` is given key that is not present in the hash, it will throw an error. But `.[]` is given key that is not present in the hash, it returns `nil`. 

Fallback: pass in a second default argument that `.fetch` will return if the key is not present in the hash.  
`person1.fetch(:middle_name, "None provided") # => "None provided"`

[Full explanation](https://chapters.firstdraft.com/chapters/767#fetch)

## Conditionals

Basic Anatomy of multibranch `if` statements:

```ruby
if condition1
  # do something if condition1 is true
elsif condition2
  # do something if condition2 is true
else # if both condition1 and condition2 were falsy
  # do something else
end
```

**Example:**

```ruby
p "Type a number less than 10 and greater than 0:"
user_input = gets.chomp.to_i # gets user input, removes newline character, converts the string to integer. 
if user_input == 5 
  p "You win!" # Will print this if the user input is 5
elsif user_input < 10 && user_input > 0 # check if the user input is valid
  p "You lose!" # Will print this if the user input is between 1 and 9
else
  p "You didn't type in a valid number." # Will print this if the user input is not between 1 and 9
end
```

**Don't forget the `end` keyword.**

[Full explanation](https://chapters.firstdraft.com/chapters/763#conditionals)

## Loops

**while statements:**  

```ruby
while boolean
  # ruby code here
end

⇒ nil
```
`while` is similar to `if`. The difference is everytime the execution of the program reaches the `end` it jumps back and evaluates the _truthiness_ of the condtion next to the `while` statement and decides whether or not to execute the code within the `while` loop.

```ruby
while condition 
  # do something while condition is true
end # jump back to the while statement
```

**Example:**

```ruby
limit = 5
while limit > 0 
  p limit
  limit = limit - 1
end 
```

> 5  
> 4  
> 3  
> 2  
> 1  

**_Note:_**  
If the condition next to the `while` always evaluates to be "truthy," then the program will be stuck in a neverending loop, infamously known as an
**infinite loop**.

[Full explanation](https://chapters.firstdraft.com/chapters/764#while-conditionally-doing-something-multiple-times)

## Blocks

### .times

```shell
Integer#times do
  # ruby code here
end

⇒ Integer
```

or

```shell
Integer#times do |Integer|
  # ruby code here
end

⇒ Integer
```

The `.times` method takes a [`Block`](https://chapters.firstdraft.com/chapters/764#blocks) as an argument and will execute the code within that block the number of times specified by the integer. A [`Block`](https://chapters.firstdraft.com/chapters/764#blocks) of code is the code written in between the keywords `do` and `end`. This looping method returns an [`Integer`](#integer) of the number of times the loop ran.

```ruby
10.times do
  p "Hi"
end
```

The above block of code will print "Hi" 10 times all on newlines. 

To keep a track of the iteration number, `.times` can create a [block variable](https://chapters.firstdraft.com/chapters/764#block-variables) that starts of counting the iteration number starting at _zero_. After each execution of the code within the block, the block variable is incremented by 1.  

```ruby
10.times do |counter|
  p counter
end
```

The above block of code will print the numbers 0 to 9 all on newlines.

[Full explanation](https://chapters.firstdraft.com/chapters/764#blocks)

#### Other methods

### .upto

```shell
Integer#upto do |Integer|
  # ruby code here
end

⇒ Integer
```
The `upto` method takes the first `Integer` the method is called on and uses it to initialize the value of
the block variable. The second `Integer` becomes the stopping condition to the loop as the block variable'
increases by one after each iteration. The method returns an [`Integer`](#integer); the initial value of the block variable. 

```ruby
5.upto(10) do |counter|
  # do something
end
```

The above block of code starts the block variable `counter` at 5 and executes the block until counter is 10.

### .downto

```shell
Integer#downto do |Integer|
  # ruby code here
end

⇒ Integer
```
The `downto` method takes the first `Integer` the method is called on and uses it to initialize the value of
the block variable. The second `Integer` becomes the stopping condition to the loop as the block variable'
decreases by one after each iteration. The method returns an [`Integer`](#integer); the initial value of the block variable.

```ruby
10.downto(5) do |counter|
  # do something
end
```

The above block of code starts the block variable `counter` at 10 and executes the block until counter is 5. 

### .step

```shell
Integer#step(Integer, Integer) do |Integer|
  # ruby code here
end

⇒ Integer
```
The `step` method initializes the block variable to be the value of the `Integer` that called the method. The first `Integer` argument is the the value the block variable is when the loop will stop. The last `Integer`argument is what value to modify the block variable  after each iteration. This method returns the [`Integer`](#integer) that called the method. 

```ruby
1.step(10, 3) do |counter|
  p counter
end
```

> 1  
> 4  
> 7  
> 10  

The above block of code starts the block variable `counter` at 1 and executes the block until counter is 10 but after each iteration the `counter` will be incremented by 3 instead of 1. `.step` can also be used to decrement the counter by a certain value. 

```ruby
10.step(1, -4) do |counter|
  p counter
end
```

> 10  
> 6  
> 2  

## Looping through Arrays

### .each

```shell
Array#each do |Object|
  # ruby code here
end

⇒ Array
```

Given an array, the `.each` method will loop through each element of the array starting with the very first one.

Returns the [`Array`](#array) the method was called on.

```ruby
cities = ["Chicago", "LA", "NYC"]
cities.each do |city|
  p city
end
```

```ruby
"Chicago"  
"LA"  
"NYC"
```

The block variable `city` holds the value of the elements in the array `cities`. It starts with the first element `"Chicago"` and then changes with each interation, holding the value of the next element (`"LA"`) in the array and so on.

[Full explanation](https://chapters.firstdraft.com/chapters/765#arrays-each-method)

### .each_with_index

```shell
Array#each_with_index do |Object, Integer|
  # ruby code here
end

⇒ Array
```

To keep a track of the iteration number while looping through an array, `.each_with_index` creates an additional block variable that starts of counting the iteration number starting at _zero_. After each execution of the code within the block, the block variable is incremented by 1.  

```ruby
cities.each_with_index do |city, count|
  p count.to_s + " " + city
end
```

```ruby
"0 Chicago"  
"1 LA"  
"2 NYC"
```

`city` holds the value of elements in the array `cities`. `count` holds the index of the element that `city` currently holds. 

**_Note:_**  
Variables created as a block variables can only be used within that block (between `do` and `end`). Using that variable outside that block will throw an error.

[Full explanation](https://chapters.firstdraft.com/chapters/765#each_with_index)
