# `Array`

Finally, we have a very important class: `Array`. Most of what we do as web
developers is manage _lists of things_. Lists of photos, likes, followers,
reviews, listings, messages, whatever.

The first structure we're going to learn to help us manage lists is `Array`.
Here's how they work:

```ruby
a = [8, 3, 1, 19, 23, 3]
```

-   The way we create `String`s by wrapping the string of characters in
    double-quotes, we create `Array`s by wrapping the list of elements in square
    brackets.
-   Seperate elements in the list with commas.

## Methods

### `count`
Counts all elements in the list and can count the occurrences of an argument.
<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055402/5e1552549fe9e9f23f0dddab47c099ac"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055583/c12a04f9499595923047a48b7780f4ba"></iframe>

### `reverse`
<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055408/abcf14392a218e7ffb87bccdb83752b2"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055729/b0a7a5e4e9aefc1f901e468b66e72170"></iframe>

### `sort`
<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055728/b398992e3f51660438e503f22d985cd1"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055739/27223a5c670df74bf54e1768e4e720aa"></iframe>

### `shuffle`
<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055405/295d1cd4b80df9827aad2cef7ac007a5"></iframe>

### `sample`
<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055406/7a15d3da296bedb370187b70d80e42bc"></iframe>

### `min`
<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055731/69cd6c4071c6dc599274cdc5d1863439"></iframe>

### `max`
<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055734/f9ea7e01a947d243dfb385bd90148a55"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056042/7591baf04059bc88bdb3ff4ca1dd0a58"></iframe>

### `sum`
<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055735/3fe14df42cfb520b9934263b223a3e8d"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056044/a9ab190d99bac78dae3ac95dbb523719"></iframe>

### `at`
Accesses an element in the array at the position given as an argument.
Positioning in an array starts at 0. Given an array of `[21, 7, 99, 34, 13]`,
`21` is in position 0, `7` is in position 1, `99` is in position 2, and so on...
<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055738/b7780eb0fe565890a511c584589ebc5e"></iframe>

### `[]`
This is a shorthand syntax for `at()`.
<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055741/ec9e6ebd2e001e9f69d121db230df8c7"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056046/2a38280792b66bcfd9b18610c1bc7b81"></iframe>

### `push`
<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055415/d3fd3e80f9cb08788c2a7abaedba3bdf"></iframe>

<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3056050/12ad63f6ff8f0591d49eb11bcf3d73fb"></iframe>

### `count`
<iframe frameborder="0" width="100%" height="600px" src="https://repl.it/student_embed/assignment/3055402/5e1552549fe9e9f23f0dddab47c099ac"></iframe>

## To Learn More

That's a fun selection of some common methods, but obviously there are a lot
more. To learn more, usually my MO is: start building something, run into a
situation where I need a new tool, like: "How do I find what spot in an array a
particular element is at?"

Then, you should, in order:

-   Post a question on Piazza. You are brand new to programming and the answers
    that you will find on the internet will oftentimes be advanced or just flat
    out wrong, and you won't yet be able to tell the difference easily. So for
    now, lean on us and each other.
-   Check the Ruby Docs for whichever class you are wondering about. In this
    case, the [Array docs](https://ruby-doc.org/core-2.2.0/Array.html). Scan the
    method list on the left until you see something that looks kinda right, and
    then check out the examples. (In this case,
    [#index](https://ruby-doc.org/core-2.2.0/Array.html#method-i-index) sounds
    about right.)
-   Google it and hope to find a helpful blog post or Stack Overflow answer.
-   Trial and error! The life of a developer.

## Next Up

Alright! It's been fun playing around with Ruby in a console, but how
would we actually builds some real, [permanent programs](permanent-programs.md)
that we can share with our friends? That's what we'll see next.
