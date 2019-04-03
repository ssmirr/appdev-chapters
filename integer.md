# Integer

Ruby differentiates between whole numbers, or `Integer`s, and decimal numbers, or `Float`s.

 ```ruby
7.class # => Integer
7.0.class # => Float
```

(In fact, you can always call the method `.class` on _any_ object, ever, at any time, to ask it what class it is.)

We'll learn about integers first.

## Methods

Let's experiment with some common methods for `Integer`s:

### + - * / % ** (math)

We, of course, have the standard math methods, like the calculator language. These methods all have the same syntactic sugar that the `String` version did, so we can say `12 + 5` rather than `12.+(5)` (thankfully).

Try each of the following:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/add-subtract-multiply-divide-exponent?lite=true"></iframe>

```ruby
12 + 5
12 - 5
12 * 5
12 / 5
```

Whoa! Did you get what you expected for that last one?

It turns out that in Ruby, the `/` operator does _whole number_ division, and will only return the whole part. If you want the remainder, you have to use the `%` (called the _modulus_) operator. Try this:

```ruby
12 % 5
```

Another maybe unexpected thing: raising a number to a power, e.g. 3<sup>2</sup>, is not done using the `^` like in many other computing environments. Instead, use the double-star `**` operator:

```ruby
3**2
```

Test your skills:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056038/bc4202d3de486a78da3f03a1d73bf638"></iframe>

### odd? and even?

The `odd?` and `even?` method return `true` or `false` based on whether the number is, well, odd or even. Don't be thrown off by the question mark at the end of the method name — it's nothing special, just another letter. Rubyists like to end method names with a question mark when the method returns `true` or `false`.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/odd?lite=true"></iframe>

### rand

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/rand?lite=true"></iframe>

### to_s

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055720/b67932171a15ce3ded5b4872d3771b7f"></iframe>
