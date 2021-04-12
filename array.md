# Array

Next, we have a _very_ important class: `Array`. Most of what we do as
developers is manage _lists of things_. Lists of photos, likes, followers,
reviews, listings, messages, rides, events, concerts, projects, etc etc etc.

The first data type we're going to learn to help us manage lists of things is
`Array`. This class, unlike the ones we've seen until now, is really just a
_container_ for other objects, and can hold however many objects we want.

## Creating arrays

As always, we have the formal way of creating a new object in Ruby: use the `.new` method on the parent class:

```ruby
cities = Array.new
```

Try it out and see what you get if you `p cities`:

[Click here for a REPL to try it.](https://repl.it/@raghubetina/Arraynew?lite=true){:target="_blank}

### push

As you can see, Ruby represents an `Array` with square brackets, `[]`. The brand new array is empty; let's add some **elements** to it with the `.push` method. Try this:

```ruby
cities = Array.new

cities.push("Chicago")
cities.push("NYC")
p cities

cities.push("LA")
# Add some of your own, if you like
p cities
```

Now we're talking! We've stored multiple strings within a single array using the `.push` method[^push_alias]. Ruby separates the elements in an array with commas.

[^push_alias]:
    You might come across a shorthand for `.push`, the `<<` method, known as the "shovel" operator. This allows you to write something like:

    ```ruby
    cities.<<("Chicago")
    ```

    Or with the syntactic sugar that we're very accustomed to by now:

    ```ruby
    cities << "Chicago"
    ```

    I personally prefer `.push` — I think it's more readable — but feel free to use the shovel if you like it better.

### Array literals

Much like with `String`s, there's a shortcut to creating `Array`s. Rather than starting with `Array.new` and building it up from scratch one `.push` at a time, we can type an array "**literal**" directly into the code:

```ruby
cities = ["Chicago", "NYC", "LA"]
```

This is the technique that we'll be using most often.

## Methods

Now let's familiarize ourselves with some of `Array`s methods.

### at

After adding elements to the list, the next most important thing we need to be able to do is retrieve an element back out from the list. Our first tool for doing that is `.at`.

The `.at` method takes an `Integer` argument, which is interpreted as the position, or **index**, in the `Array` of the element that you want to retrieve. Give it a try:

```ruby
cities = ["Chicago", "NYC", "LA", "SF", "NOLA"]

p cities
p cities.at(2)
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/array-at?lite=true){:target="_blank}

Whoa! Did you expect `cities.at(2)` to return `"LA"`? I sure didn't, the first time I tried it; I was expecting `"NYC"`.

It turns out that pretty much every programming language indexes the elements in an array starting at _zero_, not at one. So the first element is retrieved with `cities.at(0)`, the second element with `cities.at(1)`, etc. You'll get used to it after a while.

A couple of other things for you to experiment with:

 - What happens when you use an index greater than the length of the array?

    This is our first contact with `nil`, an object that represents **the absence of anything**. When you use an index "outside" the array, you might have expected to see an error message; but instead, Ruby returns `nil`.
 - What happens when you use a negative index?

### at shorthand, []

There's a shorthand for `.at()` which is very common, so you should be familiar with it. It's the `.[]` method, so we _could_ write:

```ruby
cities = ["Chicago", "NYC", "LA", "SF", "NOLA"]

p cities.[](2) # => "LA"
```

You guessed it — there's some syntactic sugar coming up. When a class has a method named `.[]`, Ruby allows the dot to be dropped, but the method name has to then move immediately next to the object it's being called on (no space), and the argument moves _inside_ the method name! Altogether, this allows us to write:

```ruby
cities = ["Chicago", "NYC", "LA", "SF", "NOLA"]

p cities[2] # => "LA"
```

Which is sort of nice. I prefer `.at` because I think it reads better, but feel free to use the square brackets if you like that style better.

```ruby
array = [8, 3, 1, 19, 23, 3]

p array[2]
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/array-square-bracket?lite=true){:target="_blank}

### first, last

Since retrieving the elements at positions `0` (the first one) and `-1` (the last one) is so common, there are handy shortcut methods for those: `.first` and `.last`. Give them a try.

### index

The `.index` method is sort of the inverse of `.at`: given an object, `.index` searches within the array and returns the index where it resides. Give it a try:

```ruby
cities = ["Chicago", "NYC", "LA", "SF", "NOLA"]

p cities.index("SF")
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/array-index?lite=true){:target="_blank}

Some further things for you to experiment with:

 - What will `.index` return if the element is present in the array more than once?
 - What will `.index` return if the element is not present in the array at all?

### String#split

Before we proceed with more `Array` methods, I want to go back for a minute and talk about [the `.split` method from the `String` class](https://chapters.firstdraft.com/chapters/757#split){:target="_blank"}. This method, when called on a `String`, will return an `Array` of substrings:

```ruby
"alice bob carol".split # => ["alice", "bob", "carol"]
```

If you provide no argument, the string is split upon whitespace, which is handy for e.g. turning a sentence into a list of words:

If you do provide an argument to `.split`, then the string will be chopped up wherever that argument occurs instead of whitespace — for example, use `"4,8,15,16,23,42".split(",")` to split on commas.

You can also `split` with the empty string, `""`, as an argument in order to turn a string into an `Array` of its individual characters:

```ruby
a = "Hello!".split("") # => ["H", "e", "l", "l", "o", "!"]
a.at(0) # => "H"
a.at(-1) # => "!"
```

This is particularly handy for us because it allows us to get a `String` of input from users with `gets` and then transform it into an `Array` for processing:

```ruby
p "Enter a series of numbers, separated by spaces:"

user_string = gets.chomp

user_numbers = user_string.split

length = user_numbers.count

p user_string
p user_numbers
p "You entered " + length.to_s + " numbers."
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/gets-with-split?lite=true){:target="_blank}

We'll be using this technique for the remainder of our test REPLs, to make things more interesting.

### count

`.count` counts how many elements are in the list, if called with no arguments. If an argument is provided, it counts how many times that argument occurs in the list.

```ruby
a = [8, 3, 1, 19, 23, 3]

p a.count

p a.count(3)
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/count?lite=true){:target="_blank}
 
### include?

A thin convenience layer on top of `.count`, `.include?` will quickly tell you whether a value is present within an `Array`:

```ruby
a = [ "a", "b", "c" ]
a.include?("b")   # => true
a.include?("z")   # => false
```

### exclude?

Similar to `.include?`, but the opposite:

```ruby
a = [ "a", "b", "c" ]
a.exclude?("b")   # => false
a.exclude?("z")   # => true
```

### reverse

```ruby
array = [8, 3, 1, 19, 23, 3]

p array.reverse # => [3, 23, 19, 1, 3, 8]
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/array-reverse?lite=true){:target="_blank}

### sort

```ruby
array = [12, 4, 5, 13, 56, 32]

p array.sort # => [4, 5, 12, 13, 32, 56]
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/sort?lite=true){:target="_blank}

### shuffle

```ruby
array = [1, 2, 3, 4, 5]

p array.shuffle # Returns a copy of array in random order
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/shuffle?lite=true){:target="_blank}

### sample

```ruby
array = [8, 3, 1, 19, 23, 3]

p array.sample # => Returns a single random element from the array
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/sample?lite=true){:target="_blank}

### min

```ruby
a = [8, 3, 1, 19, 23, 3]

p a.min # => 1
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/min?lite=true){:target="_blank}

### max

```ruby
a = [8, 3, 1, 19, 23, 3]

p a.max # => 23
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/max?lite=true){:target="_blank}

### sum

```ruby
a = [8, 3, 1, 19, 23, 3]

p a.sum # => 57
```

[Click here for a REPL to try it.](https://repl.it/@raghubetina/sum?lite=true){:target="_blank}
