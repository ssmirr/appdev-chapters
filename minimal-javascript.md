# JavaScript for Rails Developers

## History

Oh, JavaScript. Whipped together at Netscape in 1995, back during the Browser Wars; named "Java"Script to piggy-back on the "new" hotness of Java (despite having nothing to do with Java); and, through some historical, accident won the battle to become the third standard language of the web, along with HTML (for structure) and CSS (for style).

As the only scripting language that browsers run natively, it has by default become one of the most widely used general-purpose programming languages, maybe _the_ most. However, like CSS, it can't undo mistakes in the design of the language (which was done, initially, in just 10 days) very often; in order to maintain backwards compatibility. Thankfully, also like CSS, JavaScript has made tremendous strides in the last 5-10 years in paving over the warts of the past with new features and getting them adopted across _most_ browsers so that, in 2021, it's quite nice to use.

## The very basics

For the very basics of JavaScript, I recommend this Scrimba course: [Introduction to JavaScript](https://scrimba.com/learn/introtojavascript){:target="_blank"}. You should be able to watch it on 1.5x or 2x speed. Go ahead, I'll wait.

Welcome back! What did you think?

### This ain't your first rodeo

First of all, compare your experience learning JavaScript for the first time to when you learned Ruby for the first time. How did it feel this time around to learn about variables, string interpolation, arrays, arguments, `split`, etc?

So much easier, right? This was, for me, one of the best things about learning Ruby. After learning (the friendly, welcoming) Ruby and building some applications in Rails, and through that getting a pretty good understanding of programming _concepts_ (variables, conditionals, loops, objects, methods, arguments, etc) — my second[^second_lang] language was _so much easier_, and the third was even easier still.

Conversely, every language has some distinctive characteristics about it that will change the way you _think_ and make you better at your previous languages. Welcome to being a polyglot developer 🙌🏾 

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

Ok, I know this is a contrived example, but the point is: _What if the user of the method needs some information within their block that only the method can provide?_ That sounds like a job for a block variable!

Here's what the caller of the method would really like to do — make up a name for a block variable, say `edge`, and then use it within the block:

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

To send values back through block variables, we provide them as arguments to `call()` when we execute the block:

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

The key bit: `content_instructions.call(side_character)`. This is where we're sending information that then becomes `edge` when the caller of the method makes up a name for the block variable.

And that's about all there is to know about blocks.

### Callbacks

Why have I spent all this time reviewing Ruby blocks during a lesson on JavaScript?  

Ruby blocks are an example of a general concept in computer programming known as **callbacks**. [From Wikipedia](https://en.wikipedia.org/wiki/Callback_(computer_programming)){:target="_blank"}:

> In computer programming, a callback, also known as a "call-after"[1] function, is any executable code that is passed as an argument to other code; that other code is expected to call back (execute) the argument at a given time.

Callbacks are extremely common — we've seen and used them many times before, and not just every time we used a Ruby block with `do`/`end`. Think about it:

 - From the name, it's obvious that [ActiveRecord Callbacks](https://guides.rubyonrails.org/active_record_callbacks.html) are callbacks.
 - [The `dependent: destroy` option](https://guides.rubyonrails.org/association_basics.html#options-for-belongs-to-dependent) and [the `counter_cache: true` option](https://guides.rubyonrails.org/association_basics.html#options-for-belongs-to) are ready-made ActiveRecord callbacks.
 - [ActiveRecord validations](https://guides.rubyonrails.org/active_record_validations.html) are a form of callbacks.
 - [`before_action` and `after_action` filters](https://guides.rubyonrails.org/action_controller_overview.html#filters) in controllers are callbacks
 - Even controller actions themselves (code that we write in advance and give to `routes.rb` to run later when a user visits a URL) are callbacks! Most of our work all along has been writing callbacks, if you squint.

That said, JavaScript, and particularly the sort of JavaScript that we're going to be writing (to make our pages interactive) is _really_ callback-heavy.

That's because a lot of what we're going to want to do is attach **event handlers** to elements in our HTML that will wait for the user to perform some interaction (for example, click on it, or mouse-over it), and then it is supposed to spring to life and run the _callback_ we give it (`.call()` the block, in Ruby terms).

So: let's now, finally, take a look at JavaScript — through a Rails developer's eyes.

## JavaScript for Rails Developers

### Ruby -> JavaScript [rosetta stone](https://en.wikipedia.org/wiki/Rosetta_Stone)

First, go through the following Ruby statements and the roughly equivalent translation into JavaScript and see if they all make sense to you. If not, experiment with them. To experiment, create an HTML file on your computer ([VSCode](https://code.visualstudio.com/) is a good choice for an editor), write your code in a `<script>` tag, open the file in Chrome, and look at the output in [the JavaScript console](https://developer.chrome.com/docs/devtools/console/javascript/).

Ask questions on Piazza about anything that's fuzzy.

#### Semicolon rule of thumb

Rule of thumb: put a semicolon at the end of every line that doesn't end in a curly.

#### Simple output

Ruby:

```ruby
p "howdy!"
```

JavaScript:

```js
console.log("howdy!");
```

#### Declaring local variables

Ruby:

```ruby
name = "Raghu"
p "howdy, #{name}!"
```

JavaScript:

```js
let name = "Raghu";
console.log(`howdy ${name}!`);
```

#### Defining and calling methods

Ruby:

```ruby
def greet
  puts "howdy!"
end

greet
```

JavaScript:

```js
function greet() {
  console.log("howdy!");
}

greet();
```
    
Note: in JavaScript, if you don't include the parentheses (even if there are no arguments), the function will not be executed; it will be referenced as an object (so that you can e.g. pass it as an argument to a method that takes a function as an input).

#### Methods that accept arguments

Ruby:

```ruby
def greet(name)
  puts "howdy, #{name}!"
end

greet("Raghu")
```

JavaScript:

```js
function greet(name) {
  console.log(`howdy, ${name}!`);
}

greet("Raghu");
```

#### Conditionals

Ruby:

```ruby
city = "Toledo"
if city == "Chicago"
  puts "You live in the best city ever!"
else
  puts "You live in a so-so city."
end
```

JavaScript:

```js
let city = "Toledo";
if (city === "Chicago") {
  console.log("You live in the best city ever!");
}
else {
  console.log("You live in a so-so city.");
}
```

Note: That was a triple ===, not a double ==. It's not the same as the Ruby triple ===.

Beware: There's also a JS double == (non-strict equivalence) operator. Look it up.

#### For loops

Ruby:

```ruby
for i in 2...6
  puts i * i
end
```

JavaScript:

```js
for (let i = 2; i < 6; i++) {
  console.log(i * i);
}
```

#### Methods that accept blocks of code

Ruby:

```ruby
def announce(&instructions)
  puts "********** DEAR PASSENGER **********"
  instructions.call
  puts "********** THANK YOU FOR FLYING ONE-WAY AIR **********"
end

announce do
  puts "Your flight is boarding in 5 minutes."
end
```

JavaScript:

```js
function announce(instructions) {
  console.log("********** DEAR PASSENGER **********");
  instructions();
  console.log("********** THANK YOU FOR FLYING ONE-WAY AIR **********");
}

announce(function() {
  console.log("Your flight is boarding in 5 minutes.");
});
```

#### Methods that accept arguments and blocks

Ruby:

```ruby
def announce(salutation, valediction, &instructions)
  puts "********** #{salutation} **********"
  instructions.call
  puts "********** #{valediction} **********"
end

announce("DEAR PASSENGER", "THANK YOU FOR FLYING ONE-WAY AIR") do
  puts "Your flight is boarding in 5 minutes."
end
```

JavaScript:

```js
function announce(salutation, valediction, instructions) {
  console.log(`********** ${salutation} **********`);
  instructions();
  console.log(`********** ${valediction} **********`);
}

announce("DEAR PASSENGER", "THANK YOU FOR FLYING ONE-WAY AIR", function() {
  console.log("Your flight is boarding in 5 minutes.");
});
```

#### Providing block variables

Ruby:

```ruby
def announce(&instructions)
  puts "********** DEAR PASSENGER **********"
  instructions.call("*")
  puts "********** THANK YOU FOR FLYING ONE-WAY AIR **********"
end

announce do |special_character|
  puts "#{special_character} Your flight is boarding in 5 minutes. #{special_character}"
end
```

JavaScript:

```js
function announce(instructions) {
  console.log("********** DEAR PASSENGER **********");
  instructions("*");
  console.log("********** THANK YOU FOR FLYING ONE-WAY AIR **********");
}

announce(function(specialCharacter) {
  console.log(`${specialCharacter} Your flight is boarding in 5 minutes ${specialCharacter}`);
});
```

#### Methods with arguments, blocks, and block variables

Ruby:

```ruby
def announce(salutation, valediction, &instructions)
  puts "********** #{salutation} **********"
  instructions.call("*")
  puts "********** #{valediction} **********"
end

announce("DEAR PASSENGER", "THANK YOU FOR FLYING ONE-WAY AIR") do |special_character|
  puts "#{special_character} Your flight is boarding in 5 minutes. #{special_character}"
end
```

JavaScript:

```js
function announce(salutation, valediction, instructions) {
  console.log("********** " + salutation + " **********");
  instructions("*");
  console.log("********** " + valediction + " **********");
}

announce("DEAR PASSENGER", "THANK YOU FOR FLYING ONE-WAY AIR", function(specialCharacter) {
  console.log(specialCharacter + " Your flight is boarding in 5 minutes. " + specialCharacter);
});
```

#### Arrays

Ruby:


```ruby
fruits = ["apples", "oranges", "bananas"]
puts fruits[1]
puts fruits.length
```

JavaScript:

```js
let fruits = ["apples", "oranges", "bananas"];
console.log(fruits[1]);
console.log(fruits.length);
```

#### Challenge

Translate the following Ruby into Javascript:

```ruby
def for_each_in(array, &instructions)
  for i in 0...array.length
    instructions.call(array[i])
  end
end

fruits = ["apples", "oranges", "bananas", "pears"]
for_each_in(fruits) do |f|
  puts "I like #{f}."
end
```

If you can do it, you're in great shape for what we want to do — adding interactivity to our apps with **A**synchronous **J**avaScript **A**nd **X**ML — AJAX! (No one really uses XML for it anymore, but the name sounds much better than "AJAJ" (for JSON) or "AJAH" (for HTML), so we stuck with that.)
