# The most minimal introduction to JavaScript

## History

Oh, JavaScript. Whipped together at Netscape in 1995, back during the Browser Wars; named "Java"Script to piggy-back on the "new" hotness of Java (despite having nothing to do with Java); and, through some historical, accident won the battle to become the third standard language of the web, along with HTML (for structure) and CSS (for style).

As the only scripting language that browsers run natively, it has by default become one of the most widely used general-purpose programming languages, maybe _the_ most. However, like CSS, it can't undo mistakes in the design of the language (which was done, initially, in just 10 days) very often; in order to maintain backwards compatibility. Thankfully, also like CSS, JavaScript has made tremendous strides in the last 5-10 years in paving over the warts of the past with new features and getting them adopted across _most_ browsers so that, in 2021, it's quite nice to use.

## The very basics

For the very basics of JavaScript, I recommend this Scrimba course: [Introduction to JavaScript](https://scrimba.com/learn/introtojavascript){:target="_blank"}. You should be able to watch it on 1.5x or 2x speed. Go ahead, I'll wait.

Welcome back! What did you think?

### This ain't your first rodeo

First of all, compare your experience learning JavaScript for the first time to when you learned Ruby for the first time. How did it feel this time around to learn about variables, string interpolation, arrays, arguments, `split`, etc?

So much easier, right? This was, for me, one of the best things about learning Ruby. After learning (the friendly, welcoming) Ruby and building some applications in Rails, and through that getting a pretty good understanding of programming _concepts_ (variables, conditionals, loops, objects, methods, arguments, etc) — my second[^second_lang] language was _so much easier_, and the third was even easier still.

Conversely, every language has some distinct top_characteristics about it that will change the way you _think_ and make you better at your previous languages. Welcome to being a polyglot developer 🙌🏾 

[^second_lang]: This has hiring implications. _Really_ good developers can get up to speed on a new language pretty quickly (especially if it's a nice language like Ruby). If there are other developers on the team to answer questions about the codebase/framework while they are working on small introductory tasks, then it shouldn't be a dealbreaker that a strong developer has 0 years of experience in whatever stack you happen to be using, if they say they're willing to learn.

    Conversely, if a developer pigeonholes themselves as just a Rails developer or just a Node developer or just a React developer, then that's a red flag; they probably aren't very strong. (My opinion.)

## A review of Ruby blocks

I'm going to give you another brief introduction to JavaScript in a moment, with a specific focus on what we're going to need for our objective: making our web apps interactive and _slick_.

But first, it will be helpful to make sure you understand the following Ruby, so think about it carefully and [experiment with the concepts in a workspace](https://github.com/appdev-projects/base-ruby/generate){:target="blank"} if you need to.

### Defining methods that accept blocks

What if I wanted you to define a method called `border` such that I could send it some code within a block, and it would first print a row of dashes, then run the code, then print another row of dashes. So, if I called the method like this:

```ruby
border do
  p "howdy!"
  p "My name is Raghu"
end
```

When run, the output of the program should look like this:

```
----------------------------------------
howdy!
My name is Raghu
----------------------------------------
```

Could you define such a method? Go ahead, give it a try. I'll wait. (It might be helpful to review [the `about_blocks.rb` Ruby Koan](https://github.com/appdev-projects/ruby-koans/blob/9a498bdc3f78cb1b13f7ba97235c5f6c2377f60d/about_blocks.rb){:target="_blank"}.)

```ruby
# Define border here.

border do
  puts "howdy!"
  puts "My name is Raghu"
end
```

---

---

---

What we need here is to define a method that is capable of receiving not just an argument, which we're used to doing with parentheses, but a **block**.

In Ruby, we actually don't have to do anything at all to our method in order to allow it to receive a block; if a block is provided by the user of the method, it will automatically be captured and we (the method authors) can invoke it with `yield`:

```ruby
def border
  puts "-" * 40
  yield
  puts "-" * 40
end

border do
  puts "howdy!"
  puts "My name is Raghu"
end
```

However, we can also define methods that [receive and name blocks explicitly](https://github.com/appdev-projects/ruby-koans/blob/9a498bdc3f78cb1b13f7ba97235c5f6c2377f60d/about_blocks.rb#L85){:target="_blank"} by treating the block like the last argument but prefixing it with an ampersand (`&`):

```ruby
def border(&content_instructions)
  puts "-" * 40
  content_instructions.call
  puts "-" * 40
end

border do
  puts "howdy!"
  puts "My name is Raghu"
end
```

The `.call` method is used to actually execute the code that is stored in the block, whenever and wherever we're ready to.

### Defining methods that accept blocks and arguments

Okay, what about if we want the method to accept both a block _and_ arguments?

What if I wanted to improve the methods such that the caller gets to choose the size and character of the the border:

```ruby
border(20, "*") do
  puts "howdy!"
  puts "My name is Raghu"
end
```

And the output should be:

```
********************
howdy!
My name is Raghu
********************
```

Give it a try.

---

---

---

In order to allow our method to receive arguments as well, we need to move our block over and make sure it comes last:

```ruby
def border(size, top_character, &content_instructions)
  puts top_character * size
  content_instructions.call
  puts top_character * size
end
```

### Sending block variables back to the caller

Let's say, for argument's sake, that we're going to upgrade the method with side borders; something like this:

```ruby
border(10, "*") do 
  puts "Hi"
end
border(50, "=") do 
  puts "there"
end
border(20, ".") do 
  puts "world"
end
```

Which produces this:

```
**********
!        !
Hi
!        !
**********
==================================================
:                                                :
there
:                                                :
==================================================
....................
!                  !
world
!                  !
....................
```

The `border` method now adds a row above and below the content with a left and right border, but the character used is random. I would like to prepend and append the same character before and after my content, but I need the method to tell me what it's going to be.

What if the user of the method needs some information within their block that only the method can provide? That sounds like a job for a block variable! Here's what I'd really like to do:

```ruby
border(40, "*") do |edge|
  puts "#{edge} " + "howdy!".ljust(36) + " #{edge}"
  puts "#{edge} " + "My name is Raghu".ljust(36) + " #{edge}"
end
```

To produce:

```
****************************************
:                                      :
: howdy!                               :
: My name is Raghu                     :
:                                      :
****************************************
```

How do we as the authors of methods send information back through block variables? How would you write the method now? Give it a try.

---

---

---

To send back block variables, we provide them as arguments to `call()` when we execute the block:

```ruby
def border(size, top_character, &content_instructions)
  side_character = ["|", ":", "!", "I"].sample
  
  puts top_character * size
  puts side_character + " " * (size - 2) + side_character
  
  content_instructions.call(side_character)

  puts side_character + " " * (size - 2) + side_character
  puts top_character * size
end

border(40, "*") do |edge|
  puts "#{edge} " + "howdy!".ljust(36) + " #{edge}"
  puts "#{edge} " + "My name is Raghu".ljust(36) + " #{edge}"
end
```

And that about all there is to know about blocks.

### Callbacks

Why have I spent all this time reviewing Ruby blocks during a lesson on JavaScript?  


https://en.wikipedia.org/wiki/Callback_(computer_programming)



def border(&content_instructions)
  puts '-'*80
  content_instructions.call
  puts '-'*80
end






### Ruby `for` loop

Rubyists tend to prefer looping with instance methods that are well-suited to whichever situation we find ourselves in, like `Array#each`, `Integer#times`, `Integer#upto`, `Integer#step`, etc.

That said, almost all languages, Ruby included, have a general-purpose `for` keyword (along with `while`) for looping. It works like this:

```ruby
for i in 2...6
  p i * i
end
```

When run, would output:

```
4
9
16
25
```




