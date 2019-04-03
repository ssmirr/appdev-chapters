# String

Let's start by getting a more formal introduction to our friend, `String`.

First of all, notice that I when I refer to Ruby classes, I capitalize the first letter. The _only_ time we use capital letters when we're programming is when we refer to Ruby classes. All other times — variable names, file names, etc — we're going to use lowercase letters only (other than when we're writing some copy inside a string, of course).

## Creating strings

We've actually been taking a shortcut this whole time when we've been saying something like

```ruby
s = "Hello, world!"
```

In Ruby, the formal way to create a new object is to use the `.new` method on the parent class:

```ruby
s = `String`.new
```

This will, however, just give us back an empty string `""`. We would then have to add each character to it one by one. One way to do so is by using the `.concat` method, which accepts an integer [ASCII](http://www.asciitable.com/){:target="_blank"} code as an argument, translates it into a single character, and adds it on to the end of the original string:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056236/3c6d24e0123cbf5688e28063b6c552c5"></iframe>

What a pain! Now that we're clear that under hood, even creating a string is `noun.verb`, let's never do it again; from now on, we'll use the shortcut of creating _string literals_ in place within quotes. `"Thank goodness!"`

These kinds of exceptions to the regular grammar in order to make life easier are known as **syntactic sugar**.

## Methods

Next, let's familiarize ourselves with some of `String`'s methods. For each method below, we've provided some REPLs; some of them are just there for you to experiment with the code, click "run ▶", and see how the methods work; but in other ones **you need to use the methods you're reading about to make some tests pass**.

**Make sure you click "Submit" for any REPL that has tests in order to get credit for your work!**

### concat, a.k.a. +

We've already met the `.concat` method. `.concat` can accept an integer as an argument, which it interprets as an ASCII code, translates into a single character, and adds to the original string:

```ruby
"hi".concat(33) # => "hi!"
```

`.concat` can also accept a string literal as an argument, in which case it just adds the whole thing to the end of the original string.

```ruby
"hi".concat(" there") # => "hi there"
```

There's also a shorthand for `.concat`: `.+`.[^concat_lie] That may look a little funny, but it's nothing special, really; it's just a method with a very short (one letter long) name:

[^concat_lie]: This is not _quite_ true. The `+` method is not just an alias for `concat` — they do different things. But they're close enough, for our purposes.

```ruby
"hi".+(" there") # => "hi there"
```

But here's where it gets interesting; Ruby has another bit of nice syntactic sugar for us. If a class has a method named `+`, then you are allowed to drop the `.` before the method name when you call it, and just say:

```ruby
"hi" +(" there") # => "hi there"
```

Crazy! And, as we learned earlier when we were [introducing](https://chapters.firstdraft.com/chapters/755#make-the-invisible-visible){:target="_blank"} the `p` method, Ruby also allows you to omit the parentheses around arguments if you want to; so this can be further shortened to:

```ruby
"hi" + " there" # => "hi there"
```

Now this is really starting to look familiar! It's a lot like the calculator language, actually. [Developer happiness](https://chapters.firstdraft.com/chapters/755#developer-happiness){:target="_blank"}, indeed.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3054739/1cc4addd0db0fc551a3150b705aec540"></iframe>

### * (string multiplication)

`String`s can be multiplied by numbers, but the order matters:

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055223/0d5863acb6b355c53573fb78e13c0107"></iframe>

See what happens when you try:

```ruby
3 * "Hello"
```

Read The Error Message ([RTEM](https://chapters.firstdraft.com/chapters/754#seriously-please-read-the-error-message){:target="_blank"})!

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/upcase?lite=true"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055761/b8091d3eabe958cc55c0dd0d0845ec75"></iframe>

### upcase

The upcase method returns a copy of the `String` with all lowercase letters replaced with their uppercase counterparts.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055582/03f0b9912e42550cc2f5871b06e7a4c7"></iframe>

### downcase

The downcase method returns a copy of the `String` with all uppercase letters replaced with their lowercase counterparts.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055585/368b0578faa55ae7f025a9808e51d885"></iframe>

### swapcase

The swapcase returns a copy of the `String` with uppercase letters replaced with lowercase counterparts, and vice versa.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055713/54ad2cc82183a62b5d62b5c61d14dbe5"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3054730/4e44347737416ad6d635474a0e3369f1"></iframe>

### reverse

The reverse method returns a new `String` with the characters from the `String` in reverse order.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3054840/6d6e7c37386f001d85ca9a6949c722c7"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3054730/4e44347737416ad6d635474a0e3369f1"></iframe>

### length

The length method  returns the number of characters (as an `Integer`) that a `String` has.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3054853/4064e68cc52741fb36ad36bcd0f97b81"></iframe>

### chomp

The `chomp` method is mostly used to remove the `"\n"` (newline) character from the end of a string, if it is present. This seemingly strange task is very common due to the way that getting user input works; usually someone has to type something at a prompt and then they press <kbd>return</kbd> to submit it, and that adds a newline to the end of the string that they typed. Typically, we want to `chomp` that off of their input before we do anyything with it.

`chomp` can also remove other specified character(s) from the end of the string, if they are provided as an argument.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055034/4d88518a794de6d0b8b0bdbd50e2012d"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055768/5eff4e7c735dc2a2821e719a87c82c36"></iframe>

### gsub

The gsub method returns a copy of the `String` it was called on with all occurrences of the first argument substituted for the second argument.

An argument is an input to a method. In the file on the left, "ll" and "ww" are arguments that are passed to the method that affect how the method works.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055040/35043a91cfe78e61421921e1f0a437ce"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055200/37255ba7d887c35597b69abd8c7cdc9d"></iframe>


```ruby
my_string.gsub(/\s+/, "")
```

This is an advanced technique that removes all whitespace.

```ruby
gsub(/[^a-z0-9\s]/i, "")
```

This is an advanced technique that removes everything except alphanumerics and whitespace.

### to_i

Sometimes you have a string that contains a number, usually input from a user, and want to do math on it. `to_i` will attempt to convert a `String` object into an `Integer` object.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055570/9e0d4bf6a7c462730fb6af44c352bc36"></iframe>

### strip

`strip` removes all leading and trailing whitespace.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055572/55951eda552514e8e7ec0b70dcbaf8c9"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056024/e3e79ed5afa47389e0299abf09e3369f"></iframe>

### capitalize

capitalize returns a `String` with the first character converted to uppercase and the remainder to lowercase.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055716/b14358704094311fe491d83c5834474e"></iframe>

### split

This transforms the `String` into an `Array`, which we'll read more about later. By default, the string is split upon whitespace, but you can also provide an argument (e.g. `","` to split on commas).

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056028/5081a3ff7359af302bb4574448801dda"></iframe>

## More on adding strings together

We spend a lot of time composing strings of output for our users, so let's see a few more examples. Try this:

```ruby
message = "Your lucky number for today is " + rand(100) + "."
```

You'll see that Ruby gets confused, because we are trying to add an integer to a string and it doesn't feel comfortable with that.

The solution is to tell the integer to convert itself to a string first:

```ruby
message = "Your lucky number for today is " + rand(100).to_s + "."
```

The above technique for composing strings, adding them together with `+`, is called **string concatenation**.

There's another technique for composing strings that I personally find a bit easier; it's called **string interpolation**. It goes like this:

```ruby
message = "Your lucky number for today is #{rand(100)}."
```

Basically, inside the string, you place `#{}` where you eventually want your value to go. Inside the curly braces, you can write any Ruby expression without worrying about whether it is a string or not. The expression will be evaluated, converted to a string, and added to the string right in that spot. You can interpolate as many expressions as you want into a single string. Pretty neat!

If you find interpolation confusing, feel free to just use concatenation.

## Conclusion

That's about all we'll need to know about strings to do most anything related to web applications! Next, we'll take a look at numbers.
