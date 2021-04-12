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

We, of course, have the standard math methods, like the calculator language. These methods all have the same syntactic sugar that the `String` versions did, so we can say `12 + 5` rather than `12.+(5)` (thankfully).

Try each of the following:

```ruby
12 + 5
12 - 5
12 * 5
12 / 5
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/add-subtract-multiply-divide-exponent?lite=true){:target="_blank"}

Whoa! Did you get what you expected for that last one?

It turns out that the `Integer` version of division will only return another `Integer`, and so `/` only returns the whole part (like in elementary school). If you want the remainder, you have to use the `%` (called the "modulus") operator. Try this:

```ruby
12 % 5
```

Another maybe unexpected thing: raising a number to a power, e.g. 3<sup>2</sup>, is not done using the `^` like in many other computing environments. Instead, use the double-star `**` operator:

```ruby
3 ** 2
```

### odd? and even?

The `.odd?` and `.even?` methods return `true` or `false` based on whether the number is, well, odd or even. Don't be thrown off by the question mark at the end of the method name — it's nothing special, just another letter. Rubyists like to end method names with a question mark when methods return `true` or `false`.

```ruby
p 7.odd?
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/odd?lite=true){:target="_blank"}

### rand

There's another special method like `p` that we are allowed to call "in space", i.e. not on the right side of a dot[^rand_implicit_receiver], called `rand`. It returns a random number, and is very useful for all kinds of stuff, everything from games to statistical analysis:

[^rand_implicit_receiver]: This is another method defined on `Kernel`, so the longhand would be `Kernel.rand(6)`.

```ruby
rand(6) # => returns a random integer between 0 and 5
```

Somewhat oddly, `rand(n)` will return a random integer between `0` and `n - 1` rather than between `1` and `n`. That may seem surprising, but it's actually pretty handy because a lot of times what we want to do is generate a random _offset_ and it's convenient for that to include 0 as a possibility.

Give it a try:

```ruby
# random number between 0 and 9
p rand(9)
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/rand?lite=true){:target="_blank"}

### to_s

We often will want to combine our `Integer`s with `String`s when crafting output for our users. Give it a try:

```ruby
lucky_number = rand(100)
p "Your lucky number is" + lucky_number
```

Uh oh! [RTEM!](https://chapters.firstdraft.com/chapters/754#seriously-please-read-the-error-message){:target="_blank"}

It turns out that `String`'s `+` method can only add two strings together, not a string and an object of some other class. So, a lot of times we'll need to convert an `Integer` into a `String` prior to output. Fortunately `Integer` has a handy method, `to_s` (or "to string"), that does just that:

```ruby
p 98.to_s
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/tos?lite=true){:target="_blank"}

### to_f

Similarly, there's a `to_f` (or "to float") method to convert an `Integer` to a `Float`, which is often handy for doing math, as we'll see next.

## Conclusion

That's it for `Integer`. Next up, [it's close cousin: `Float`](https://chapters.firstdraft.com/chapters/759).
