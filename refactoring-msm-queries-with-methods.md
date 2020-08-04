# Refactoring MSM Queries with Methods

This chapter is the companion to [the refactoring-msm-queries-1 project](https://github.com/appdev-projects/refactoring-msm-queries-1){:target="_blank"}, which is the sequel to the [the msm-queries project](https://github.com/appdev-projects/msm-queries){:target="_blank"}.

## Objective

Our goal is to keep msm-queries working exactly the same way that it was after we finished building it; we're not going to add a thing. Therefore, we'll use the exact same target as before:

[https://msm-queries.matchthetarget.com/](https://msm-queries.matchthetarget.com/){:target="_blank"}

Our starting point code for this project, refactoring-msm-1, is one solution for msm-queries. But we're going to make the code much more modular and re-usable, while keeping the functionality exactly the same. How? By **defining methods** to encapsulate our querying logic.

You should read through the starting point code and compare it to your own solution to msm-queries. `rails sample_data` and `bin/server` so that you can click through the application, verify that it's working, and read the server log.
  
Are there any differences between the starter code and your own solution to msm-queries? You will most likely find at least one or two. What are they doing? Practice _reading_ the code and reasoning your way through it, line by line; explain it to yourself, or to your rubber ducky. Developers read far more code than we write.

Does any part of the code puzzle you?

Go ahead, I'll wait!

## The problem: queries all over the place

Right now, there are several places in our application where we have a movie and we want to display something about the director of the movie.

For example, in `app/views/movie_templates/show.html.erb`, we have an instance of `Movie` in a variable called `@the_movie`, and we want to display the name of the director. So, first, we use the value in the attribute `@the_movie.director_id` to look up a matching record in the directors table:

```erb
<% matching_directors = Director.where({ :id => @the_movie.director_id }) %>
    
<% the_director = matching_directors.at(0) %>
```

Then, finally, we can get the value in the `name` or `dob` or `bio` or whatever other attribute we care about from that instance of `Director`:

```erb
<%= the_director.name %>
```

This same basic logic — given a movie, use its `director_id` attribute to look up a row in the directors table — is repeated in several places: the movies index page, an actor's filmography, etc. If we were to continue building this application out more, no doubt we would be doing this task many more times; nearly everywhere a movie appears, or in the `rails console`, or in a rake task, along with information about the movie itself (like `title`, `year`, etc), we will want to display information about the associated director (like `name`, etc).

What a pain to have to repeat this query,

```ruby
Director.where({ :id => @the_movie.director_id }).at(0)
```

, every single time I just want some info about `@the_movie`'s director!

## The solution: encapsulate queries in methods

Let's make life easier on our future selves and on all our future teammates: **let's define a nicely named instance method** that will do this work and that we can call on any instance of `Movie` whenever we want to.

The method will encapsulate what we've been doing repetitively over and over until now: it will talk to the `Director` class, retrieve the record from the directors table corresponding to the movie's `director_id`, and return an instance of `Director` to us. Then, we can use `Director` attribute accessors to get whichever column values we are interested in about the director.

Since we named our foreign key column in the movies table `director_id`, we automatically got an attribute accessor method on `Movie` called `.director_id` from ActiveRecord which returns an `Integer`.

What shall we call our new method that we will define, which will _use_ the `Integer` returned by `.director_id` to retrieve an instance of `Director` and return that instead?

How about `.director`?

Here's what I wish we could do. If we have an instance of `Movie` inside a variable called `@the_movie`, instead of:

```erb
<% matching_directors = Director.where({ :id => @the_movie.director_id }) %>
    
<% the_director = matching_directors.at(0) %>

<%= the_director.name %>
```

I want to be able to do:

```erb
<% the_director = @the_movie.director %>

<%= the_director.name %>
```

Or, if we're okay with chaining a couple of methods on the same line, we could even do:

```erb
<%= @the_movie.director.name %>
```

Ahhhhh! Wouldn't that be nice? Having a method at our fingertips called `.director` that we can call on any instance of `Movie` whenever we want to, and it would know how to perform the database query to find the associated record and return an instance of `Director` to us? It'd be like an _association_ accessor, the way we have convenient _attribute_ accessors for our column values.

Unfortunately, if you try `<%= @the_movie.director %>` in `app/views/movie_templates/show.html.erb` right now, you'll get a big red `"undefined method 'director' for #<Movie:0x00007faadebb9e88>"` error. If we want handy "association accessors" like that, we'll have to roll up our sleeves and invent them ourselves!

## Instance method review

### Naming the method

If you feel very confident about [defining instance methods](https://chapters.firstdraft.com/chapters/769#defining-instance-methods){:target="_blank"}, then you can skip forward to .... Otherwise, read on.

Recall that, in Ruby, we use the `def` keyword within the `class` definition to add new methods to a class. Here, we're adding the `say_hi` instance method within the definition of the `Person` class:

```ruby
class Person
  def say_hi
    return "Hello!"
  end
end
```

Since the `say_hi` comes immediately after the `def` keyword[^class_method], Ruby knows we are defining an _instance-level_ method. 

[^class_method]: If, instead of `def say_hi`, we had said `def Person.say_hi`, then we would have defined a _class-level_ method.

### Returning a value

Every method's main job is _returning_ a value, which is what it gets substituted by as the program is being evaluated. So, the above class would now work like this:

```ruby
p = Person.new # create a new instance of Person
x = p.say_hi # this expression is substituted by the return value "Hello!" and stored in x
y = x.upcase # => y now contains the return value of upcase, "HELLO!"
# etc
```

Our programs are essentially just a long sequence of methods being called on the return values of previous methods, until we arrive at our desired output.

### Composing methods to build more powerful methods

Usually, we start by defining small, simple methods; but then we combine these methods into ever more powerful ones.

#### Attribute accessors

Some of the simplest but most useful methods are _attribute accessors_. In our pure Ruby days, we had to declare them ourselves (although more recently ActiveRecord has been doing it for us automatically based on column names):

```ruby
class Person
  attr_accessor(:first_name)
  attr_accessor(:last_name)
end
```

Each call to `attr_accessor` gives us two instance methods: one for storing a value in an instance of the class, and one for retrieving the value. So now, our `Person` instances can remember their first names and last names:

```ruby
class Person
  attr_accessor(:first_name)
  attr_accessor(:last_name)
end

sd = Person.new
sd.first_name = "Shreya"
sd.last_name = "Donepudi"
p sd # => #<Person:0x00007fe0c24a6eb0 @first_name="Shreya", @last_name="Donepudi">

pm = Person.new
pm.first_name = "Patrick"
pm.last_name = "McKernin"
p pm # => #<Person:0x00007fe0c23f0a70 @first_name="Patrick", @last_name="McKernin">

jw = Person.new
jw.first_name = "Jelani"
jw.last_name = "Woods"
p jw # => #<Person:0x00007fe0802ca8b8 @first_name="Jelani", @last_name="Woods">
```

#### Doing rather than reading

If you're feeling the urge to try out the Ruby that you're reading about — great!

[Click here to create a blank Gitpod workspace](http://gitpod.io/#https://github.com/appdev-projects/helloruby){:target="_blank"}. Then, create a new file, type the Ruby you want to experiment with into it, and run it from a Terminal tab with `ruby YOUR_FILENAME`.

For example, I created a file called `experiment.rb` and am running it with the Terminal command `ruby experiment.rb` to see my output. **Don't forget to turn on Autosave.**

### Using existing instance methods when defining new instance methods

Now, what if we wanted to display each person's full name? We could do this:

```ruby
p sd.first_name + " " + sd.last_name # => "Shreya Donepudi"
p pm.first_name + " " + pm.last_name # => "Patrick McKernin"
p jw.first_name + " " + jw.last_name # => "Jelani Woods"
```

(I'm omitting the class definition for brevity; `class Person` is still defined above in my `experiment.rb`, as well as the lines of code where we created the three `Person` instances and assigned their attribute values.)

But wouldn't it be nice if we had a nicely named method we could call instead, like `.full_name`?

```ruby
p sd.full_name # => undefined method `full_name' for #<Person:0x00007fe0c24a6eb0>
p pm.full_name # => undefined method `full_name' for #<Person:0x00007fe0c23f0a70>
p jw.full_name # => undefined method `full_name' for #<Person:0x00007fe0802ca8b8>
```

Let's define the method, to get one step closer to our goal:

```ruby
class Person
  attr_accessor(:first_name)
  attr_accessor(:last_name)

  def full_name
    return "Shreya Donepudi"
  end
end
```

Now, if we try again:

```ruby
p sd.full_name # => "Shreya Donepudi"
p pm.full_name # => "Shreya Donepudi"
p jw.full_name # => "Shreya Donepudi"
```

Well, it's progress, I suppose. At least we resolved the `undefined method 'full_name' for #<Person:0x00007fe0c24a6eb0>` issue. But, we want each instance to use it's _own_ first name and last name attributes to put together it's full name. How can we author the `.full_name` method to do that?

If we changed the definition of the method to the following:

```ruby
class Person
  attr_accessor(:first_name)
  attr_accessor(:last_name)

  def full_name
    assembled_name = pm.first_name + " " + pm.last_name
    
    return assembled_name
  end
end
```

Would it help? No, it wouldn't. Give it a try and see what the error message says.

```
undefined local variable or method `pm' for #<Person:0x00007fe0c24a6eb0> (NameError)
```

We can't use the `sd`, `pm`, or `jw` local variables when we _define_ the instance method. When we _author_ the method, we have no idea what the _invokers_ of the method are going to name their variables next month or next year when they _use_ the method, or whether they are going to create variables at all! Perhaps they are just going to chain this method on to the end of another method.

So, when we're authoring the `.full_name` method, we need some way to refer to whichever instance of the `Person` class the `.full_name` method is going to be called upon _in the future_. Ruby gives us a way: the `self` keyword.

#### The self keyword

You can think of `self` as a special variable that holds whatever object is at the forefront of Ruby's mind when it is evaluating a program. Whatever line of code it is reading, whatever object is it working on at the moment, that is what `self` contains.

For our purposes, here's what matters right now: when we're defining instance methods, when writing code _within_ the instance method definition, `self` represents the instance that the method will be called upon in the future.

That's great, because since we almost always need to use other, pre-existing instance methods while defining new instance methods, we need a way to refer to the object in question so that we can call those other methods on it.

So, `self` is what will allow us to use the pre-existing `.first_name` and `.last_name` attribute accessor methods to build up our handy `.full_name` method:

```ruby
class Person
  attr_accessor(:first_name)
  attr_accessor(:last_name)

  def full_name
    assembled_name = self.first_name + " " + self.last_name

    return assembled_name
  end
end
```

Now, we can use it!

```ruby
p sd.full_name # => "Shreya Donepudi"
p pm.full_name # => "Patrick McKernin"
p jw.full_name # => "Jelani Woods"
```

Yay! So handy. And, now that `.full_name` exists, we can use it along with `self` to build up other methods:

```ruby
class Person
  attr_accessor(:first_name)
  attr_accessor(:last_name)

  def full_name
    assembled_name = self.first_name + " " + self.last_name

    return assembled_name
  end

  def full_name!
    return self.full_name.upcase
  end
end
```

(Don't be fooled by the `!` at the end of the new method's name; it has no special meaning. It's just another letter, as far as Ruby is concerned. And one letter's difference makes them as distinct as `zebra` and `giraffe` in Ruby's eyes.)

## Defining "association accessors"

Okay, now that we've brushed up on how to define instance methods, let's do something practical with it.

### The goal

If we have an instance of `Movie` inside a variable called `@the_movie`, instead of:

```erb
<% matching_directors = Director.where({ :id => @the_movie.director_id }) %>
    
<% the_director = matching_directors.at(0) %>

<%= the_director.name %>
```

I want to be able to do:

```erb
<% the_director = @the_movie.director %>

<%= the_director.name %>
```

Or, if we're okay with chaining a couple of methods on the same line, we could even do:

```erb
<%= @the_movie.director.name %>
```

If you try `<%= @the_movie.director %>` in `app/views/movie_templates/show.html.erb` right now, you'll get a big red `"undefined method 'director' for #<Movie:0x00007faadebb9e88>"` error. Let's define the instance method and make it work.

### Find the class definition

All of our model class definitions are located in the `app/models` folder. Find `movie.rb` and define a method called `director`:


```ruby
class Movie < ApplicationRecord
  def director
    return "Hello!"
  end
end
```

## Conclusion

You shouldn't be worrying about writing that database query over and over and over and over again while you are crafting your user interface. Or, more realistically, on a multi-person team, the
