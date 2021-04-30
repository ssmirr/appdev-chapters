# Authorization with Pundit

Now that've got a handle on, fundamentally, how authorization is done — with conditionals and redirects — for an application of any real size, it's probably worth using an authorization library. A good one will give us helper methods that will allow us to be more concise, and more importantly will help us avoid letting security holes slip through the cracks.

## Pundit

There are many Ruby authorization libraries, but my go-to is one called [Pundit](https://github.com/varvet/pundit){:target="_blank"}. Add it to your `Gemfile` and `bundle`.

Pundit revolves around the idea of **policies**. Create a folder within `app/` called `policies/`, and within it create a file called `photo_policy.rb`. This will be a plain ol' Ruby object (PORO):

```ruby
class PhotoPolicy
end
```

The idea is that we want to encapsulate all knowledge about who can do what with a photo inside instance methods within this "policy" class. Let's set up the class to accept a user and a photo when instantiated:

```ruby
class PhotoPolicy
  attr_reader :user, :photo

  def initialize(user, photo)
    @user = user
    @photo = photo
  end
end
```

And now, for example, to figure out who can see a photo, let's define a method called `show?` that will return `true` if the user is allowed to see the photo and `false` if not:

```ruby
class PhotoPolicy
  attr_reader :user, :photo

  def initialize(user, photo)
    @user = user
    @photo = photo
  end

  # Our policy is that a photo should only be seen by the owner or followers
  #   of the owner, unless the owner is not private in which case anyone can
  #   see it
  def show?
    user == photo.owner ||
      !photo.owner.private? ||
      photo.owner.followers.include?(user)
  end
end
```

Let's test it out in `rails console`:

```irb
[1] pry(main)> alice = User.first
=> #<User id: 15>
[2] pry(main)> bob = User.second
=> #<User id: 16>
[3] pry(main)> alice.followers.include?(bob)
=> false
[4] pry(main)> alice.private?
=> true
[5] pry(main)> photo = alice.own_photos.first
=> #<Photo id: 157>
[6] pry(main)> policy_a = PhotoPolicy.new(alice, photo)
=> #<PhotoPolicy:0x00007fca9886d8e0>
[7] pry(main)> policy_a.show?
=> true
[8] pry(main)> policy_b = PhotoPolicy.new(bob, photo)
=> #<PhotoPolicy:0x00007fca5fa3a6e8>
[9] pry(main)> policy_b.show?
=> false
```

Great! Now we can replace the conditionals that we have scattered about the application with this method. 

Let's start by locking down access to the `Photos#show` action. Instead of redirecting inside a `before_action` (which is a solid strategy, mind you), let's take a slightly different tack: we'll raise an exception if the `current_user` isn't authorized.

```ruby
def show
  unless PhotoPolicy.new(current_user, @photo).show?
    raise Pundit::NotAuthorizedError, "not allowed"
  end
end
```

Test it out by visiting a show page that you shouldn't be able to (you can remove the `/edit` from an edit page to find a URL.)

Great! But what if we don't want to show an error page? Redirecting with a flash message was pretty nice. Well, we can rescue that specific exception in `ApplicationController`:

```ruby
rescue_from Pundit::NotAuthorizedError, with: :user_not_authorized

private

  def user_not_authorized
    flash[:alert] = "You are not authorized to perform this action."
    
    redirect_back(fallback_location: root_url)
  end
```

Now we have all of the same functionality back, but we're nicely set up to encapsulate all of our authorization logic in one place.

## Convention over configuration in controllers

So far, Pundit hasn't done anything for us at all, other than providing the exception class that we raised. We wrote all the Ruby ourselves. But now, let's use some some helper methods from Pundit that will allow us to be _very_ concise, if we follow conventional naming.

First, `include Pundit` in `ApplicationController` to gain access to the methods:

```ruby
# app/controllers/application_controller.rb

include Pundit
```

Now, we can use the `authorize` method. Instead of all this:

```ruby
def show
  unless PhotoPolicy.new(current_user, @photo).show?
    raise Pundit::NotAuthorizedError, "not allowed"
  end
end
```

We can write just this:

```ruby
def show
  authorize @photo
end
```

🤯 What just happened? When you pass the `authorize` method an instance of `Photo`:

 - It assumes there is a class called `PhotoPolicy` in `app/policies`.
 - It assumes there is a method called `current_user`.
 - It passes `current_user` as the first argument and whatever you pass to `authorize` as the second argument to a new instance of `PhotoPolicy`.
 - It calls a method named after the action with a `?` appended on the new policy instance.
 - If it gets back `false`, it raises `Pundit::NotAuthorizedError`.

## Views

In view templates, we now have a `policy` helper method that will make it easier to conditionally hide and show things. For example, assuming we define an `update?` method in our policy:

```erb
<% if policy(@photo).update? %>
  <%= link_to "Edit photo", edit_photo_path(@photo) %>
<% end %>
```

## Just plain ol' Ruby

Since policies are just POROs, we can bring all our Ruby skills to bear: inheritance, [aliasing](https://medium.com/rubycademy/alias-in-ruby-bf89be245f69){:target="_blank"}, etc.

To start with, we can run the generator `rails g pundit:install` which creates a good starting point policy to inherit from in `app/policies/application_policy.rb`. Take a look and see what you think. If you like it, let's inherit from it:

```ruby
# app/policies/photo_policy.rb

class PhotoPolicy < ApplicationPolicy
```

## Secure by default

So — that means that all we need to do from now on is:

 1. Remember to define a method in our policy that matches **every** action name (but we can alias).
 2. Remember to call `authorize` within **every** action.

If we've done #2, which will force us to do #1, that will ensure that we've at least thought about who can get into every action.

Try visiting `/follow_requests` right now — oops! Quite a big security hole, and one that's depressingly common to find left open after a resource has been generated with `scaffold`.

Pundit includes a method called `verify_authorized` that we can call in an `after_action` to help enforce the discipline of pruning our unused routes:

```ruby
after_action :verify_authorized
```

[There's another method called `policy_scope`](https://github.com/varvet/pundit#scopes){:target="_blank"} similar to `authorize` that's used for collections rather than single objects. Usually, you'll want to ensure that either one or the other is called with something like the following in `ApplicationController`:

```ruby
# app/controllers/application_controller.rb
after_action :verify_authorized, except: :index
after_action :verify_policy_scoped, only: :index
```

Now try visiting `/follow_requests` or some other scaffolded route that was insecurely left reachable. We are now secure-by-default instead of insecure-by-default. If necessary, you can make the choice to `skip_before_action :verify_authorized` on a case-by-case basis, as we did for `:authenticate_user!`.

## Read more

There's more to Pundit [that you should read about in the README](https://github.com/varvet/pundit){:target="_blank"}, but not a _ton_ more. That's what I like about it — it's relatively lightweight, but gets the job done well. The way that it is structured also plays great with [Rails I18n](https://guides.rubyonrails.org/i18n.html){:target="_blank"} and other libraries. Another powerful tool for your belt.
