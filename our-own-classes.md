# Ruby Classes

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

And we can declare what attributes a person can have with the `attr_accessor` keyword:

```ruby
class Person
  attr_accessor :first_name
  attr_accessor :last_name
  attr_accessor :role
end
```

And now the `Person` class is a first-class citizen in the language, just like `Array` and `Hash`:

```ruby
c = Person.new
c.first_name = "Raghu"
c.last_name = "Betina"
c.role = "Instructor"

c.last_name # => "Betina"
c.class # => Person
```

For each attribute that we declared, we get methods that we can call to assign and retrieve values.

There are a few reasons I like using classes more than `Hash`es to model things, but here is the big one: in addition to just storing a list of attributes about a thing, we can also _define our own methods_ with the `def` keyword. For example,


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

"Hello, #{hs.full_name}!" # => "Hello, Homer Simpson!"
```

Two new keywords to note:

 -  I used the `return` keyword to signify what value I wanted to replace `hs.full_name` in the original expression after it's been evaluated.
 - I used the `self` keyword to refer to the object who was asked to calculate its full name, since I can't know in advance what (if any) variable name will be used.

Here's a slightly more involved example:

```ruby
class Person
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

hs.age # => 29, as of this writing
```

So, rather than using a `Hash` to model real world things, it's a good idea to create classes, and then empower them with *behavior* (methods) in addition to information.

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

What would happen if I tried doing `person4.role`? How about `person1.grade`? Why? What would the error message be?

## Using our own classes

We usually store classes that we write in the `app/models` folder. For example, if you create a file in that folder called `person.rb` with the following:

```ruby
class Person
  attr_accessor :first_name
  attr_accessor :last_name
  attr_accessor :birthdate

  def full_name
    return self.first_name + " " + self.last_name
  end

  def age
    dob = Date.parse(self.birthdate)
    now = Date.today
    age_in_days = now - dob # Returns a Rational number
    age_in_years = age_in_days / 365

    return age_in_years.to_i
  end
end
```

You can now use the `Person` class from anywhere in the app: from within any controller, any view template, in the `rails console` -- or even from within another model.

Ruby is called an Objected Oriented (OO) language because we always strive to organize our code into descriptive classes and methods, rather than just using Hashes and Arrays for everything.

For example, Rubyists much prefer to define the `Person` class above and then

```ruby
hs = Person.new
hs.first_name = "Homer"
hs.last_name = "Simpson"

"Hello, #{hs.full_name}!" # => "Hello, Homer Simpson!"
```

Rather than

```ruby
h = { :first_name => "Homer", :last_name => "Simpson" }

"Hello, #{h.fetch(first_name)} #{h.fetch(:last_name)}!" # => "Hello, Homer Simpson!"
```

even though the two are functionally equivalent, and the second _could_ be considered more concise (in terms of number of lines of code).

By encapsulating the logic of how to compute `full_name` in the class definition, I make it much easier to re-use elsewhere and share.
