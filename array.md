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

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/Arraynew?lite=true"></iframe>

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
    You might come across a shorthand for `.push`, the `<<` method. This allows you to write something like:

    ```ruby
    cities.<<("Chicago")
    ```

    Or with the syntactic sugar that we're very accustomed to by now:

    ```ruby
    cities << "Chicago"
    ```

    I personally prefer `.push`, but feel free to use `<<` if you like it better.

### Array literals

Much like with `String`s, there's a shortcut to creating `Array`s. Rather than `Array.new` and building up from scratch one `.push` at a time, we can type array "**literals**" directly:

```ruby
cities = ["Chicago", "NYC", "LA"]
```

This is the technique that we'll be using most often.

## Methods

Now let's familiarize ourselves with some of `Array`s methods.

### count

`.count` counts how many elements are in the list, if called with no arguments. If an argument is provided, it counts how many times that argument occurs in the list.

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/count?lite=true"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055583/c12a04f9499595923047a48b7780f4ba"></iframe>

### reverse

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/array-reverse?lite=true"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055729/b0a7a5e4e9aefc1f901e468b66e72170"></iframe>

### sort

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/sort?lite=true"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055739/27223a5c670df74bf54e1768e4e720aa"></iframe>

### shuffle

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/shuffle?lite=true"></iframe>

### sample

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/sample?lite=true"></iframe>

### min

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/min?lite=true"></iframe>

### max

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/max?lite=true"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056042/7591baf04059bc88bdb3ff4ca1dd0a58"></iframe>

### sum

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/sum?lite=true"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056044/a9ab190d99bac78dae3ac95dbb523719"></iframe>

### at

Accesses an element in the array at the position given as an argument.
Positioning in an array starts at 0. Given an array of `[21, 7, 99, 34, 13]`,
`21` is in position 0, `7` is in position 1, `99` is in position 2, and so on...
<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/array-at?lite=true"></iframe>

### []

This is a shorthand syntax for `at()`.
<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/@raghubetina/array-square-bracket?lite=true"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056046/2a38280792b66bcfd9b18610c1bc7b81"></iframe>
