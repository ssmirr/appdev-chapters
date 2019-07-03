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

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/our-own-classes?lite=true"></iframe>

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
  require "date" # We need to pull in the Date class, which is not loaded by default

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

So, rather than using a `Hash` to model real world things, it's a good idea to create classes, and then empower them with *behavior* (methods) in addition to information.

### Defining class methods

The methods `full_name` and `age` above are known as _instance methods_, because we call them on individual **instances** of the `Person` class (Homer, Mickey, Minnie, etc).

We can also define **class**-level methods, that we call directly on `Person` itself. This can be handy if we want to define re-usable utility methods that don't really belong to any one individual person. A somewhat contrived example is if we wanted to be able to quickly produce first name with last initial from a full name:

```ruby
class Person
  def self.abbreviate(full)
    first_and_last = full.split
    first_name = first_and_last.at(0)
    last_name = first_and_last.at(1)
    last_name_characters = last_name.split("")
    last_name_initial = last_name_characters.at(0)
    
    return first_name + " " + last_name_initial + "."
  end
end

Person.abbreviate("Raghu Betina")
```

The new things to note in the code above:

 - We use the `self` keyword _when defining the method_ to make it a class method rather than an instance method. That way, we call the method directly on capital-`P` `Person`.
 - We give the method the ability to accept an argument by adding parentheses and choosing a name for the argument when defining the method (sort of like we choose a name for a block variable within the pipes). Then we can use that argument within the method definition, sort of like a block variable.

## Test your skills

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3108918/e76a68e1f22d348686d6bc6459a1d0fe"></iframe>

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

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/our-own-classes-inheritance?lite=true"></iframe>
