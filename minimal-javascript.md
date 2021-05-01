# JavaScript for Rails Developers

## History

Oh, JavaScript:

 - whipped together at Netscape in 1995, back during the great Browser Wars
 - named "Java"Script to piggy-back on popularity of Java (despite having nothing to do with Java)
 - and, through some historical accident, won the battle to become the third standard language of the web, along with HTML (for structure) and CSS (for style).

As the only scripting language that browsers run natively, it has by default become one of the most widely used general-purpose programming languages; maybe _the_ most.

However, like CSS, it can't undo mistakes in the design of the language[^ten_days] very often; in order to maintain backwards compatibility.

Thankfully, also like CSS, JavaScript has made tremendous strides in the last 5-10 years in paving over the warts of the past with new features and getting them adopted across _most_ browsers so that, in 2021, it's quite nice to use.

[^ten_days]: The initial design of JavaScript was done in just 10 days. This sometimes painfully obvious in the early days.

## The very basics

For the very basics of JavaScript, I recommend this Scrimba course: [Introduction to JavaScript](https://scrimba.com/learn/introtojavascript){:target="_blank"}. You should be able to watch it on 1.5x or 2x speed. Go ahead, I'll wait.

Welcome back! What did you think?

### This ain't your first rodeo

First of all, compare your experience learning JavaScript for the first time to when you learned Ruby for the first time. How did it feel this time around to learn about variables, string interpolation, arrays, arguments, `split`, etc?

_So_ much better, right? This was, for me, one of the best things about learning Ruby.

After learning (the friendly, welcoming) Ruby and building some applications in Rails, and through that getting a pretty good understanding of programming _concepts_ (variables, conditionals, loops, objects, methods, arguments, etc) — my second[^second_lang] language was _so much easier_, and the third was even easier still.

Conversely, every language has some distinctive characteristics about it that will change the way you _think_ and make you better at your previous languages. Welcome to being a polyglot programmer 🙌🏾 

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

If you can do it, you're in great shape for what we want to do — adding interactivity to our apps with **A**synchronous **J**avaScript **A**nd **X**ML — AJAX[^ajaj]!

[^ajaj]: No one really uses XML for it anymore, but the name sounds much better than "AJAJ" (for JSON) or "AJAH" (for HTML), so we stuck with AJAX 😉

## Manipulating HTML elements

JavaScript is okay, as far as languages go, but we were perfectly happy with Ruby. The only reason we're interested in JavaScript is that it has a unique ability: it can run inside the browser, and that means we can use it to modify HTML elements in our pages _without needing to refresh the page_. This is the key to making our apps feel like _apps_ and not pages.

Let's see how this works. In Chrome, open the JavaScript Console from the View > Developer menu. There, you have a REPL, much like `irb`, where you can type any JavaScript expression. (There's also great autocomplete functionality in the Chrome dev tools, so use <kbd>tab</kbd> often.)

Importantly, there is an object available, `document`, that represents the page itself:

```js
document
```

Let's try working with it:

```js
let headings = document.getElementsByTagName("h1");
```

We've created a variable, `headings`, and stored an array of all the `h1` elements on the page within it. Neat!

As you were typing, you might have noticed the autocomplete showing you other methods that were available — `getElementsByClassName`, `getElementByID`, etc. These are all ways that we can query the DOM (Document Object Model) and find HTML elements; exactly the way we did when we were writing CSS selectors.

Let's keep going and get just one heading out of the array:

```js
let h = headings[0];
```

Now that we have one element in the variable `h`, we can do things like:

 -  `h.innerText` (will return the text content of the element)
 -  `h.classList.add("display-1")` (adds a CSS class to the element)
 -  `h.remove()` (removes the element from the DOM)
 -  and a whole bunch more. Neat!

## jQuery

In 2021, plain vanilla JavaScript is pretty nice to use; but before, say, 2015, it was quite a different story. The language itself was missing lots of features that we Rubyists take for granted, there were lots of cross-browser inconsistencies, and life was hard.

Sound familiar? It was the same story with CSS; with CSS, developers came together to share their solutions to rough edges and cross-browser inconsistencies with frameworks like Bootstrap. In the JavaScript world, the go-to library to make up for JavaScript's shortcomings was jQuery.

In 2021, jQuery isn't absolutely essential to survival, the way it was for many years. Many developers have started to drop it and just use vanilla JavaScript, which does have some benefits in terms of performance/the amount of data users have to download.

However, I still use jQuery on almost every project and I recommend that you do too, because I'm not interested in dealing with the 1% of cases where there are still rough edges. Let's create a new HTML file and start to experiment with jQuery. [Here's a convenient CDN](https://code.jquery.com/) we can use to pull it in:

```html
<script src="https://code.jquery.com/jquery-3.6.0.min.js" integrity="sha256-/xUj+3OJU5yExlq6GSYGSHk7tPXikynS7ogEvDej/m4=" crossorigin="anonymous"></script>
```

### Add some elements

Let's get some HTML elements in our page to work with. (You can add the standard HTML boilerplate around it if you want to.)

```html
<script src="https://code.jquery.com/jquery-3.6.0.min.js" integrity="sha256-/xUj+3OJU5yExlq6GSYGSHk7tPXikynS7ogEvDej/m4=" crossorigin="anonymous"></script>

<h1>Cities</h1>

<ul>
  <li class="northeast">New York</li>
  <li class="west">Los Angeles</li>
  <li id="best" class="midwest">Chicago</li>
  <li class="south">Houston</li>
  <li class="west">Phoenix</li>
</ul>
```

### The jQuery function

Our primary entrypoint into the utility of the library is one function: `jQuery()`. Let me point out right away the [the official jQuery documentation](https://api.jquery.com/jquery/) is quite good, so you should refer to it often.

The `jQuery()` function is sort of like the `document.getElementsByTag()`, `document.getElementsByClassName()`, `document.getElementByID()`, etc, functions that we met earlier — but all wrapped into one. You pass in any CSS selector (as a string) as an argument, and `jQuery()` will return an array of matching elements. Try this in the JS console:

```js
jQuery("li");
jQuery(".west");
jQuery("#best");
```

Since we're going to be using the `jQuery()` function _a lot_, they conveniently aliased it as `$()`, to save us a bunch of typing. So the above could also be shortened to:

```js
$("li");
$(".west");
$("#best");
```

### DOM manipulation with jQuery

Once we've queried the DOM for the elements we're interested in, jQuery provides many methods for manipulating them. Let's try putting an element in a variable:

```js
let x = $("#best");
```

Now, we can manipulate it:

```js
x.hide();
```

Poof! Let's bring it back:

```js
x.show();
```

Or, if we're not sure what state it's in (visible or invisible) to start with, we can toggle:

```js
x.toggle();
x.toggle();
x.toggle();
```

That's pretty abrupt, though. How about something a little smoother?

```js
x.hide();
x.fadeIn();
x.fadeOut();
x.fadeToggle();
```

Or even smoooooother:

```js
x.fadeToggle(5000);
```

### Using callbacks

As I mentioned earlier, JavaScript is callback-heavy. Here's a simple example:

In the smoooooother example above, we had the element take 5 seconds to fade. What if we wanted to have something after the element faded out, but not until then? No problem! The method (like most jQuery methods) accepts a callback (the equivalent of a Ruby block):

```js
x.fadeToggle(5000, function() {
  alert("All done!");
});
```

Another common thing we'll do is bind **event handlers** to elements. Try the following:

```js
x.on("click", function() {
  $(".west").slideToggle();
});
```

Now, whenever `x` is clicked, elements with class `west` will slide up and down. Nice!

Other examples of events are `mouseover`, `submit` (for forms), `keydown` and `focus` (for inputs).

#### Frequently used jQuery methods

Read about and experiment with the following methods. If you need to, refresh the page to reset the state of the elements:

 - [https://api.jquery.com/hide/](https://api.jquery.com/hide/)
 - [https://api.jquery.com/show/](https://api.jquery.com/show/)
 - [https://api.jquery.com/toggle/](https://api.jquery.com/toggle/)
 - [https://api.jquery.com/remove/](https://api.jquery.com/remove/)
 - [https://api.jquery.com/append/](https://api.jquery.com/append/)
 - [https://api.jquery.com/prepend/](https://api.jquery.com/prepend/)
 - [https://api.jquery.com/slideUp/](https://api.jquery.com/slideUp/)
 - [https://api.jquery.com/slideDown/](https://api.jquery.com/slideDown/)
 - [https://api.jquery.com/fadeToggle/](https://api.jquery.com/fadeToggle/)
 - [https://api.jquery.com/replacewith/](https://api.jquery.com/replacewith/)
 - [https://api.jquery.com/html/](https://api.jquery.com/html/)

## Conclusion

And that, from the perspective of a Rails developer who wants to improve the UX of their web application, is all we _need_ to know about JavaScript — for the moment.

Now, we're ready to revamp the interactions in our UI. When someone clicks on a link or submits a form, in many cases (usually after an action that CRUDs some records in some table(s)) we end up redirecting them to the same URL that they were on before; but they wind up at the top of the page, losing their scroll position. Annoying!

Let's see if we can use our new JavaScript skills to improve this experience — with a technique called Ajax. We're going to:

 1. Break the default behavior of links and forms, so when the user clicks on them, they don't go anywhere.
 2. Make the GET/PATCH/POST/DELETE request to whatever route that the link or form would have made in the background, using JavaScript.
 3. From the action, respond with a bit of jQuery instead of with a `.html.erb` template.
 4. The jQuery will run in the user's browser to update just the part of the page that needs it (adding the comment, removing the like, etc).
 5. Voilá! Ajax!

That's coming up next.
