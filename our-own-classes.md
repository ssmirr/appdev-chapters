# Our own classes

## Modeling real world things

We can *model* a person pretty well with an `Array`:

```ruby
a = Array.new # This is the longhand for a = []
a.push("Raghu")
a.push("Betina")
a.push("Instructor")

a # => ["Raghu", "Betina", "Instructor"]

a.at(1) # => "Betina"
a.class # => Array
```

And we can model a person even better with a `Hash`:

```ruby
h = Hash.new # This is the longhand for h = {}
h.store(:first_name, "Raghu")
h.store(:last_name, "Betina")
h.store(:role, "Instructor")

h # => { :first_name => "Raghu", :last_name => "Betina", :role => "Instructor" }

h.fetch(:last_name) # => "Betina"
h.class # => Hash
```

But we can do even better than a `Hash`. We can define *our own class* to represent people with the `class` keyword:

```ruby
class Person
end
```

Remember, the only time we use capital letters in Ruby is when we're referring to classes — and this goes for when we're naming our own classes, too. So `class person` will not work. If you have a multi-word name, then `CamelCase` it — `class VeryImportantPerson`.

And we can declare what attributes a person can have with the `attr_accessor` keyword:

```ruby
class Person
  attr_accessor :first_name
  attr_accessor :last_name
  attr_accessor :role
end
```

And now the `Person` class is a first-class citizen in the language, just like `Array` and `Hash`. Compare the code below to the code above for creating `Array`s and `Hash`s to store information:

```ruby
class Person
  attr_accessor :first_name
  attr_accessor :last_name
  attr_accessor :role
end

c = Person.new
c.first_name = "Raghu"
c.last_name = "Betina"
c.role = "Instructor"


p c.last_name # => "Betina"
p c.role
p c.class # => Person
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/our-own-classes?lite=true)

For each attribute that we declared, we get methods that we can call to assign and retrieve values.

### Defining instance methods

There are a few reasons I like using classes more than `Hash`es to model things, but here is the big one: in addition to just storing a list of attributes about a thing, we can also _define our own methods_ with the `def` keyword. For example, try adding the following `full_name` method to the class we defined in the REPL above:

```ruby
class Person
  attr_accessor :first_name
  attr_accessor :last_name

  def full_name
    return self.first_name + " " + self.last_name
  end
end
```

Now, in addition to being able to store data (first and last names), I can ask any `Person` to compute its full name:

```ruby
hs = Person.new
hs.first_name = "Homer"
hs.last_name = "Simpson"

"Hello, " + hs.full_name + "!" # => "Hello, Homer Simpson!"
```

Two new keywords to note:

 -  I used the `return` keyword to signify what value I wanted to replace `hs.full_name` in the original expression after it's been evaluated.
 - I used the `self` keyword to refer to the object who was asked to calculate its full name, since I can't know in advance what (if any) variable name will be used.

Here's a slightly more involved example:

```ruby
class Person
  require("date") # We need to pull in the Date class, which is not loaded by default

  attr_accessor :birthdate

  def age
    dob = Date.parse(self.birthdate)
    now = Date.today
    age_in_days = now - dob
    age_in_years = age_in_days / 365

    return age_in_years.to_i
  end
end
```

Now every `Person` that we create will have the ability to compute their age based on their own dob attribute:

```ruby
hs = Person.new
hs.birthdate = "April 19, 1987"

hs.age # => 32, as of this writing
```

Note that we had to `require("date")`[^require] in order to load the `Date` class into the program; Ruby doesn't load this class into every program by default, like it does with the core classes (`String`, `Integer`, etc).

[^require]: The parentheses are almost always dropped after `require`.

So, rather than using a `Hash` to model real world things, it's a good idea to create classes, and then empower them with *behavior* (methods) in addition to information.

### Defining class methods

The methods `full_name` and `age` above are known as _instance methods_, because we call them on individual **instances** of the `Person` class (Homer, Mickey, Minnie, etc).

We can also define **class**-level methods, that we call directly on `Person` itself. This can be handy if we want to define re-usable utility methods that don't really belong to any one individual person.

Here's an example similar to `Date.parse` — what if we wanted users of the `Person` class to quickly be able to create new instances of the class like this:

```ruby
Person.parse("Betina, Raghu")

# => should return a new person with first
#    and last name attributes already populated
```

Then, we can define the **class-level** method `parse`, called directly on `Person`, like this:

```ruby
class Person
  attr_accessor :first_name
  attr_accessor :last_name

  def Person.parse(last_comma_first)
    last_first_array = last_comma_first.split(",")
    the_last_name = last_first_array.at(0).strip
    the_first_name = last_first_array.at(1).strip
    
    a_new_person = Person.new
    a_new_person.first_name = the_first_name
    a_new_person.last_name = the_last_name
    
    return a_new_person
  end
end
```

The new things to note in the code above:

 - When defining the method, we do `def Person.parse` rather than just `def parse` to make it a **class method** rather than an **instance method**. That way, we call the method directly on capital-`P` `Person`.
 - We give the method the ability to accept an argument by adding parentheses and choosing a name for the argument when defining the method. Then we can use the input within the method definition, sort of like how we use a block variable.

## Inheritance

When you define new classes, you can choose to inherit all the power of a "parent" class, and then add some custom behavior:

```ruby
class Instructor < Person
  attr_accessor :role
end

class Student < Person
  attr_accessor :grade
end
```

`Instructor`s and `Student`s can do everything people can, and a little bit more.

Creating the first individual **instance** of the `Instructor` class:

```ruby
person1 = Instructor.new
person1.first_name = "Raghu"
person1.last_name = "Betina"
person1.role = "Lecturer"
```

Creating the second individual instance of the `Instructor` class:

```ruby
person2 = Instructor.new
person2.first_name = "Arjun"
person2.last_name = "Venkataswamy"
person2.role = "Faculty Coach"
```

Creating the first individual instance of the `Student` class:

```ruby
person3 = Student.new
person3.first_name = "Trenton"
person3.last_name = "Arthur"
person3.grade = "A"
```

Creating the second individual instance of the `Student` class:

```ruby
person4 = Student.new
person4.first_name = "Tom"
person4.last_name = "Besio"
person4.grade = "Incomplete"
```

Now we can use them:

```ruby
person1.full_name # => "Raghu Betina"
person1.role # => "Lecturer"
person2.full_name # => "Arjun Venkataswamy"
person2.role # => "Faculty Coach"
person3.full_name # => "Trenton Arthur"
person3.grade # => "A"
person4.full_name # => "Tom Besio"
person4.grade # => "Incomplete"
```

What would happen if I tried doing `person4.role`? How about `person1.grade`? Why? What would the error message be? Try defining all of the above and give it a shot in a REPL:

```ruby
class Person
end

class Instructor
end

class Student
end
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/our-own-classes-inheritance?lite=true)
