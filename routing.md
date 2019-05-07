# Routing

Our users place **requests** by visiting URLs in our app. In order to respond to requests _dynamically_ (as opposed to sending back _static_, unchanging pages of HTML), we need to **route** each request to a Ruby method that can do the desired work (reading from an API, picking a random number, CRUDing from the database, etc) and then send back the output (formatted in HTML[^jsonapi]).

[^jsonapi]: The format of the response could also be JSON, PDF, CSV, XML, or others — and we can offer multiple formats at once. If we wanted to add an iPhone or Android app, for example, we would add a second format — JSON — and then let the native app read our API.

Our apps are essentially defined by the list of URLs that we allow users to visit, and for which we provide responses. We keep this list of URLs in one _very_ important file: `config/routes.rb`.

Here's an example `routes.rb`:

```ruby
# /config/routes.rb

Rails.application.routes.draw do
  match("/rock", { :controller => "game", :action => "play_rock", :via => "get" })
  match("/paper", { :controller => "game", :action => "play_paper", :via => "get" })
  match("/scissors", { :controller => "game", :action => "play_scissors", :via => "get" })
  match("/", { :controller => "game", :action => "homepage", :via => "get" })
end
```

## Route

 - All of our routes must be contained within the block following `Rails.application.routes.draw`. A new Rails app will already come with this code pre-written in `routes.rb`.
 - Each route is comprised of the `match` method and its two arguments:
    - The first argument to `match` is a `String`: the _path_ that we want users to be able to visit (the path is the portion of the URL that comes after the domain name).
    - The second argument to `match` is a `Hash`: this is where we tell Rails which method to call when a user visits the path in the first argument. (We'll have to actually write this method in the next step, after we write the route.)

        The `Hash` must have three key/value pairs:

        - `:controller`: The value for this key is what we're going to name the _class_ that contains the method we want Rails to call when the user visits the path.
        - `:action`: The value for this key is the what we're going to name the method itself. "Action" is the term used to refer to Ruby methods that are triggered by users visiting URLs.
        - `:via`: The value for this key must be either `"post"`, `"get"`, `"patch"`, or `"delete"`.

            In HTTP (the protocol for exchanging resources over the internet), these are the names for CREATE, READ, UPDATE, and DELETE, respectively; and are known as "HTTP verbs".

            We use the `:via` key to specify which CRUD operation best describes the work we're going to perform in the method. It turns out that _most_ requests involve _reading_ info, rather than creating/updating/deleting it; so we'll use `"get"` most often. If you can't think of exactly which one to use, just default to `"get"`.

## Controller

Suppose we wrote a route in `config/routes.rb` that looks like this:

```ruby
match("/rock", { :controller => "game", :action => "play_rock", :via => "get" })
```

Now when a user visits `/rock`, instead of seeing a "No route matches" error, they will instead see an error `uninitialized constant GameController`. Progress!

As we know, when Ruby says "uninitialized constant" it means "I can't find that class".

So, what's going on here? When we said `:controller => "game"` in the route, we _told_ Rails to look for a class called `GameController` when someone visits `/rock`.

 - All of the controller class names will end in `...Controller`, and they will begin with whatever value we provided for the key `:controller` in the route.
 - Like all Ruby classes, the name must be `CamelCase` (not `snake_case` or `Some_Hybrid`). So in this case, it will be `GameController`.
 - The class must be defined in a Ruby file that is the `snake_cased` version of its name. Rails will itself use the `.underscore` method to figure out the name; we can try it ourselves in `rails console`:

    ```ruby
    [2] pry(main)> "GameController".underscore
    => "game_controller"
    ```
 - The Ruby file must be placed within the `app/controllers/` folder. So, in this case, we create a file called `app/controllers/game_controller.rb` (don't forget the `.rb` file extension).
 - Finally, within this file, we define the class:

    ```ruby
    class GameController < ApplicationController
    end
    ```
 - We inherit from a Rails base class called `ApplicationController`, much like our models inherited from `ApplicationRecord`. Our models inherited `.save`, `.where`, and a bunch of other awesome database-related methods from `ApplicationRecord`; whereas our controllers are going to inherit a bunch of methods like `render`, `redirect_to`, and a bunch of other awesome request/response-related methods from `ApplicationController`.
 - Don't forget the `end` that goes with the `class`; type it before you forget it.
 - Now, when a user visits the path `/rock`, the "uninitialized constant" error should go away. Progress!

    If you still see the "unitialized constant" error, then:

    - You named your class wrong; it must exactly match the value in `routes.rb`, followed by `Controller` (singular), and `CamelCase`.
    - You named the file wrong. Try doing `.underscore` on a string containing the class name in `rails console` to figure out the correct filename.
    - You put the file in the wrong folder. It has to be within `app/controllers/`. Not within, for example, `app/` or `app/controllers/concerns/`.
    - You forgot the `.rb` file extension.
    - If you can't find which of the above it is, try deleting what you did and paving over your work again from scratch. Sometimes you just can't spot your own typos, and paving over is the best approach.

## Action

The next error the user will encounter is:

```ruby
The action 'play_rock' could not be found for GameController
```

This is good! That means we successfully defined `GameController`. Now we have to define a method within it called `play_rock`, since that's what we specified as the value for the `:action` key back in our route. Let's do it:

```ruby
class GameController < ApplicationController
  def play_rock
  end
end
```

The primary job of an _action_ is to send back a response to the user — either some HTML for their browser to render, or perhaps redirecting them to some other URL. We get a method for each of these two: `render` for the first, and `redirect_to` for the second.

### redirect_to

Let's try `redirect_to` first:

```ruby
class GameController < ApplicationController
  def play_rock
    redirect_to("https://www.google.com")
  end
end
```

As you can see, the argument to `redirect_to` is a string which contains some absolute URL that you want the user to simply be forwarded to. This will come in handy later when, for example, we want to send the user back to an index page of photos after they've deleted a photo.

### render

More commonly, however, we'll use `render` to send some HTML to the user's browser for display:

```ruby
class GameController < ApplicationController
  def play_rock
    render("game_templates/user_plays_rock.html.erb")
  end
end
```

The argument to `render` is a string that contains the name of a folder and a file that we're going to place some HTML in, which we want to be sent to the user's browser when they visit `/rock`.

 - We can choose any folder and file name that we want — I just made up the folder name of `game_templates` and the file name of `user_plays_rock`. These files are known as our **view templates**.
 - The extension of the file should be `.html.erb`. The `.erb` stands for **embedded Ruby**, and this is what is going to allow us to make our HTML dynamic and awesome instead of static and boring.

## View

If we've defined our action correctly, the next error a user sees when they try to visit `/rock` is:

```
Missing template game_templates/user_plays_rock.html.erb with {:locale=>[:en], :formats=>[:html], :variants=>[], :handlers=>[:raw, :erb, :html, :builder, :ruby, :arb, :coffee, :jbuilder]}. Searched in:
  * "/home/ubuntu/workspace/app/views"
```

We're almost there! We just have to create this file. Notice from the error message that Rails searched within the folder `app/views/`. This is where we'll store all of our view templates. Let's create the folder and file we decided upon, `app/views/game_templates/user_plays_rock.html.erb`.

Within that file, you know what to do:

```erb
<p>Hello, world!</p>
```

Now, finally, when a user visits `/rock`, they should see a page and no more errors. We've connected the Route-Controller-Action-View (**RCAV**) dots successfully!

## ERB

The point of all of this work, as opposed to just creating a file called `public/rock.html`, is that we can now _embed Ruby_ inside our HTML:

```erb
<p>You played rock!</p>

<p>The computer played <%= ["rock", "paper", "scissors"].sample %>!</p>
```

At last — our game is finally dynamic! The `<%=  %>` (embedded ruby tags, or ERB tags) allow us to write _any Ruby expression we want_ and it will be evaluated, converted into a string, and injected into the source code of the HTML document before the HTML is sent to the browser. The browser will never know that Ruby was involved (you can verify this by Viewing Source of the final page in your live application preview).

There's also another version of the ERB tag, `<%  %>` (without the equal sign) that does _not_ inject the value into the source of the HTML; this is useful for doing some computation or control flow with conditionals:

```erb
<p>You played rock!</p>

<% computer_move = ["rock", "paper", "scissors"].sample %>

<p>The computer played <%= computer_move %>!</p>

<% if computer_move == "rock" %>
  <p>You tied!</p>
<% elsif computer_move == "paper" %>
  <p>You lost!</p>
<% else %>
  <p>You won!</p>
<% end %>
```
