# Refactoring Fortune Teller with Dynamic Routes

This chapter is the companion to [the refactoring-fortune-teller project](https://github.com/appdev-projects/refactoring-fortune-teller), which is the sequel to the [the fortune-teller project](https://github.com/appdev-projects/fortune-teller).

## Part 1: Dice

Our starting point code for refactoring-fortune-teller is the target code for fortune-teller. So, in the code you're starting with, the horoscopes from Part 2 of fortune-teller have already been debugged, and the dice from Part 3 of fortune-teller have already been built out.

In refactoring-fortune-teller, our goal is to keep everything working exactly the same way that it is; we're not going to add much. But we're going to get rid of 90% of the lines of code, while keeping the functionality the same. How? With **dynamic route segments**. Consider our new target:

[https://refactoring-fortune-teller.matchthetarget.com/roll/2/6](https://refactoring-fortune-teller.matchthetarget.com/roll/2/6){:target="_blank"}

Seems like the same thing that we had before, right? Well, try this URL instead:

[https://refactoring-fortune-teller.matchthetarget.com/roll/42/1337](https://refactoring-fortune-teller.matchthetarget.com/roll/42/1337){:target="_blank"}

That's a lot of 1337-sided dice. Try any combination of two numbers you want in the second and third segments of the path. You'll see that they all work!

Obviously, I didn't just happen to add routes for the exact pairs of numbers you chose. Instead, there's only _one_ route handling all of the dice rolls; it matches URLs that starts with `/roll/` and have two more segments, but allows _any_ values to appear in the last two segments.

In the action, Rails allows us to examine what values were in those spots, and customize the response accordingly. How do we get access to the values? Through our old friend, the `params` hash! Let's see how:

---

First, in your own app, try visiting a URL like this:

```
/roll/2/6
```

It should work. First, follow the R→C→A→V through and familiarize yourself with the code.

---

Now, try visiting a URL like this:

```
/roll/42/513
```

You will get a "no route matches" error.

Let's first make it so that users can visit _any_ URL of the form `/roll/X/Y` and it will not throw an error, no matter what `X` and `Y` are:

If we set up a route like this (for now let's send it to the existing `two_six` action):

```ruby
get("/roll/:zebra/:giraffe", { :controller => "dice", :action => "two_six" })
```

and then try visiting `/roll/42/513` again, the route _does_ match, and you get sent to the old action for two six-sided dice. What happens if you change the route to:

```ruby
get("/roll/zebra/giraffe", { :controller => "dice", :action => "two_six" })
```

(removing the colons before `zebra` and `giraffe`) and visit `/roll/42/513` again? "No route matches." Ok, put the colons back:

```ruby
get("/roll/:zebra/:giraffe", { :controller => "dice", :action => "two_six" })
```

So, **by beginning a segment of a path with a colon, we're making it _dynamic_**. Rails will, for the purpose of routing, allow _anything_ there; it's like a wildcard.

Now, watch your server log while visiting the URL:

```bash
Parameters: { "zebra" => "42", "giraffe" => "513" }
```

It's just like when we got inputs from the query string! Except, in this case, the keys in the hash were determined by us in advance, in the route; rather than being tacked on to the end of the URL itself by the visitor (or a form).

Try visiting the following paths:

 - `/roll/alice/bob`
 - `/roll/42/513/alice`
 - `/42/513`

Which ones throw a "no route matches" error and which ones don't? What appears in the `params` hash in your server log?[^try_it]

[^try_it]: Did you actually go visit those URLs? Why not? Do or do not; there is no read.

Okay, from here, we `.fetch` the data from the `params` hash, just as we did before when learning about query strings. Let's make up a new action, instead of messing with `two_six`; and let's use more meaningful labels for the dynamic segments than `:zebra` and `:giraffe`:

```ruby
get("/roll/:number_of_dice/:how_many_sides", { :controller => "dice", :action => "infinity_and_beyond" })
```

Now, when we visit `/roll/42/513`, we get a "missing action" error, as expected. Let's define the `infinity_and_beyond` action and make it say "hi" as usual:

```ruby
# app/controllers/dice_controller.rb

class DiceController < ApplicationController
  def infinity_and_beyond   
    render({:template => "dice_templates/infinity.html.erb"})
  end

  # ...
```

```erb
<%# app/views/dice_templates/infinity.html.erb %>

<h1>hi</h1>
```

Test it out and make sure you wired up your R→C→A→V correctly; if not, read the error messages and debug.

Now, let's `.fetch` the information from the `params` hash that we need to make this response intelligent:

```ruby
# app/controllers/dice_controller.rb

class DiceController < ApplicationController
  def infinity_and_beyond
    @num_dice = params.fetch("number_of_dice")
    @num_faces = params.fetch("how_many_sides")

    render({:template => "dice_templates/infinity.html.erb"})
  end

  # ...
```

I made the variables instances variables instead of local variables, by starting them with `@`, under the assumption that I will want to use them in the view template.

And we can use logic similar to the logic in all the other actions to actually create an array of "dice rolls" and draw them in the template:

```ruby
class DiceController < ApplicationController
  def infinity_and_beyond
    @num_dice = params.fetch("number_of_dice")
    @num_faces = params.fetch("how_many_sides")

    @array_of_rolls = Array.new

    @num_dice.to_i.times do    
      @array_of_rolls.push(rand(@num_faces.to_i) + 1)
    end

    render({:template => "dice_templates/infinity.html.erb"})
  end

  # ...
```

```erb
<%# app/views/dice_templates/infinity.html.erb %>

<h1>
  <%= @num_dice %>d<%= @num_faces %>
</h1>

<ul>
  <% @array_of_rolls.each do |a_roll| %>
    <li>
      <%= a_roll %>
    </li>
  <% end %>
</ul>
```

And, voilà, it should work — and it should work for any combination of dice and faces! _Including_ the 18 combinations that we had before. Comment out all the old routes and click on all the links in the nav — they still work!

Gleefully, delete all the old routes, actions, and view templates — we love deleting code. That's less costly code to maintain.

But wait! Are you sure you didn't forget and break some old functionality that we had before? How can we be _sure_?

A question to ponder.

---

## Part 2: Horoscopes

Let's try to do something similar for the zodiacs; that is, let's see if we can use a dynamic route segment to reduce the amount of code.

It won't be quite as straightforward as with the dice, because there's actual data (the horoscopes) that isn't the same on each page; it's not as formulaic as generating random numbers based on the ones that are in the URL.

But, if we had a way to _look up_ the horoscope given the astrological sign that's in the URL, then the rest of every action would be the same. Generate lucky numbers, and populate a template that's the same for every action.

Imagine you had a hash of hashes that had all of the signs and horoscopes within it, something like an API response — then could you make a single RCAV that supported all 12 zodiacs? If you had a hash like this available to you:

```ruby
{
  :aries => {
    :name => "Aries",
    :horoscope => "As your professional dreams unfold, Aries, you may worry about the downside. First, there are new responsibilities that you might doubt your ability to fulfill. Second, you might be catapulted into an uncomfortable new realm of office politics. Don't let these matters put a damper on your enthusiasm. You have what it takes to fulfill the first concern and the wisdom to avoid the second. Onward and upward."
  },
  :leo => {
    :name => "Leo",
    :horoscope => "Success on all levels is filling your life and making you feel absolutely wonderful, Leo. The downside of this is that you might be a little too conscientious. Are you putting in a lot of extra hours? Be discriminating about this and don't work harder than necessary. You could get stressed to the point of taxing your strength too much, and that won't help you. Pace yourself."
  },
  # etc
}
```

Fortunately, you do. I've created a class called `Zodiac`, and I gave it class-level method called `list` which returns that exact hash above[^could_you].

[^could_you]: Can you define a class and a class-level method like this? You ought to be able to, with what you've learned about [Our own classes](https://chapters.firstdraft.com/chapters/769) and [Hashes](https://chapters.firstdraft.com/chapters/767).

Give the `Zodiac` class a try. For example, in the `FireController#ram` action, you can replace:

```ruby
@horoscope = "As your professional dreams unfold, Aries, you may worry about the downside. First, there are new responsibilities that you might doubt your ability to fulfill. Second, you might be catapulted into an uncomfortable new realm of office politics. Don't let these matters put a damper on your enthusiasm. You have what it takes to fulfill the first concern and the wisdom to avoid the second. Onward and upward."
```

With:

```ruby
all_zodiacs = Zodiac.list
this_zodiac = all_zodiacs.fetch(:aries)
@horoscope = this_zodiac.fetch(:horoscope)
```

Verify that your `/zodiacs/aries` action still works just like before. If you're curious, the `Zodiac` class is in the `app/models` folder — you can go have a look at it, if you like. When we want to define a new Ruby class within a Rails app, we create a file for it `app/models` folder. The filename is important — it should exactly match the class name (just like when we create controller classes).

In this case, I created a file called `zodiac.rb` within `app/models/`, and boom — _everywhere_ within the Rails application (all controllers, all views, even within other models), we have access to the `Zodiac` class; and we can call `Zodiac.list`.

We'll talk a lot more about models in the coming weeks, but for now you can just use the `Zodiac.list` method I provided; just know that it returns a hash of hashes with the keys as shown above. You know how to work with hashes.

----

Now, your task is: with what you've learned about dynamic route segments, and with the ability to look up a horoscope given a sign, can you replace all 12 of the R→C→A→Vs for zodiacs with just one R→C→A→V?

```ruby
get("/zodiacs/:the_sign", { :controller => "fortunes", :action => "horoscopes" })
```

You can pick whatever controller and action name you like, but when you're done the application should function exactly the same as before.

### String#to_sym

If we try to access an element in a `Hash` that looks like this:

```ruby
zodiacs = {
  :aries => "stuff",
  :leo => "more stuff",
}
```

with a `String` key `"leo"`:

```ruby
zodiacs.fetch("leo")
```

we'll get the familiar error message:

```
KeyError (key not found: "leo")
```

because the `String`, `"leo"`, is not the same as the `Symbol`, `:leo`.

Remember that all values in the `params` hash come to us as `String`s. Just as we had to convert these `String`s to `Float`s with `.to_f` before doing math on them in Omnicalc, we'll have to convert them to `Symbol`s if we want to use them to key into the `Hash` returned by `Zodiac.list`.

Fortunately, there's a handy method `.to_sym` that will do just that:

```ruby
"hello".to_sym # => :hello
```

With the `.to_sym` method in hand, you should be able to use `params` and `Zodiac.list` to replace all 12 actions with 1!

## Regressions

How can you be _sure_ that it functions exactly the same as before? Well, in this case, since we had automated tests for Part 2, you can run `rails grade`! Isn't it a nice, safe feeling to be able to run the tests to know that you didn't introduce any [regressions](https://en.wikipedia.org/wiki/Software_regression#:~:text=A%20software%20regression%20is%20a,change%20to%20daylight%20saving%20time){:target="_blank"}?

## Refactoring

What we just did is called **refactoring**:

> In computer programming and software design, **code refactoring** is the process of restructuring existing computer code—changing the factoring—without changing its external behavior. Refactoring is intended to improve the design, structure, and/or implementation of the software (its _non-functional_ attributes), while preserving its functionality. Potential advantages of refactoring may include improved code readability and reduced complexity; these can improve the source code's maintainability and create a simpler, cleaner, or more expressive internal architecture or object model to improve extensibility.
>
> Wikipedia, "[Code refactoring](https://en.wikipedia.org/wiki/Code_refactoring){:target="_blank"}" 

Refactoring is a crucial part of the software development process. First, we write messy, clunky, repetitive, but easy to understand and most importantly _functional_ code; then, only after having wrapped our heads around the problem by _solving_ it, we sit back and take a moment to think about whether there might be a more readable or less complex or more performant solution.

However, once we have a working solution, it's often very tempting to just leave it alone; why mess with a good thing and risk introducing bugs? Especially if you're dealing with a large, old, complicated system that you didn't build entirely yourself; it can be irresponsible to refactor willy nilly if you don't fully understand what you're changing (see [Chesterton's fence](https://en.wikipedia.org/wiki/Wikipedia:Chesterton%27s_fence){:target="_blank"}).

And yet, we do need to make changes to our codebase over time; even if we're satisfied to never refactor, we at some point _have_ to add new features or make security patches. How do we make sure we don't inadvertently break anything or introduce bugs? No matter how good our quality assurance team is, it's not realistic to expect them to manually examine every user path through the app and verify that it still works, with every combination of possible inputs, every single time any developer makes any change.

The answer: **automated tests**. This is why developers invest so much time and effort in writing automated test suites (this is what you've been running every time you do a `rails grade`). Automated tests are nothing more than a separate Ruby script that, essentially, web scrapes our own app, clicks every link, fills out every form, with every possible combination of inputs, and makes sure that the app is doing the right thing under all scenarios. As you might imagine, this self-web-scraping script takes a long time to write; often, longer than a feature itself.

But, if you plan to maintain a codebase over the long term (not always true for e.g. a throwaway prototype), it is well worth the investment.

## Optional extra challenge

Can you refactor RPS RCAV to use only one action instead of three for the URLs `/rock`, `/paper`, and `/scissors`?
