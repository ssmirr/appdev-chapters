# draft_generators

## Installation

Make sure this line is in your `Gemfile`:

```ruby
# /Gemfile

gem "draft_generators", :git => "https://github.com/firstdraft/draft_generators", :branch => "winter-2020"
```

Then, at a Terminal prompt:

```bash
bundle install
```

Or make sure it is up to date with:

```bash
bundle update draft_generators
```

If you are starting with our [base-rails repository](https://github.com/appdev-projects/base-rails), then you've already got the gem included.

## Generating a resource

To generate a complete, database-backed web resource:

```bash
rails generate draft:resource <MODEL_NAME_SINGULAR> <COLUMN_1_NAME>:<COLUMN_1_DATATYPE> <COLUMN_2_NAME>:<COLUMN_2_DATATYPE> # etc
```

For example,

```bash
rails generate draft:resource photo image:string caption:text owner_id:integer
```

- The model name must be singular.
- Separate column names and datatypes with colons (NO SPACES).
- Separate name:datatype pairs with spaces (NO COMMAS).

In other words, the format of the command is exactly the same as when you were [generating only a model and table](https://chapters.firstdraft.com/chapters/770#the-quick-way-to-create-a-table){:target="_blank"}, but `draft:model` is replaced with `draft:resource`.

> Note: `rails g` is short for `rails generate`, like  `c` is for `console` and `s` is for `server`.

As usual, we have to run the command `rails db:migrate` at a Terminal prompt to execute the migrations that were written for us by the generator, to actually create the new table.

### What just happened?

Whoa, what just happened? Well, let's look at the output from the command:

```bash
create  app/controllers/photos_controller.rb
invoke  active_record
create    db/migrate/20200602191145_create_photos.rb
create    app/models/photo.rb
create  app/views/photos
create  app/views/photos/index.html.erb
create  app/views/photos/show.html.erb
  route  RESTful routes
insert  config/routes.rb
```

 - A model and a migration for the table.
 - A controller named after the table.
 - Some view templates related to the table.
 - Some routes related to the table.

If you look at the routes, controller, and views, you'll see that it has essentially added in all of the standard CRUD boilerplate for us — automatically! Try navigating to `/photos`, or whatever your table is called, and you'll see that you have a fully-functional interface to CRUD records. Awesome!

This makes sense because, as you might have noticed by now, the Golden Five RCAVs relating to letting users Create, Read, Update, and Delete records through their browser are basically the same for every table; only the form inputs vary, really, based on the different columns in the tables.

And since we're going to need most of the Golden Five for most of our tables, why not automate the boilerplate? Then we can just delete the stuff that we don't want, and modify what's left to match our needs.

Whew! What a time saver. However, it only saves time if you are comfortable with all of the code that it is writing for you — otherwise the code is intimidating and you are scared to change it. Then, rather than saving you time, the generators slow you down; relative to you writing code by hand that you understand 💯. So choose your approach wisely. When in doubt, pick a URL that you want the user to be able to visit, connect the RCAV dots yourself, and make it happen.

### What if I made a mistake when I generated a resource?

#### If you've already run rails db:migrate

If you made a mistake when generating your `draft:resource` AND you've _already_ run `rails db:migrate`, then first you have to run the command:

```bash
rails db:rollback
```

to get the database back into the state that it was in before. The command looks at the most recent migration and reverses it. If you haven't run `rails db:migrate` yet, then you can proceed to `rails destroy`, below.

#### rails destroy draft:resource

Anything that you can `rails generate`, you can also `rails destroy`; the latter command will undo whatever the former one does. So, for example, you can:

```bash
rails destroy draft:resource photo
```

This will remove all the files that `rails generate draft:resource photo ...` had previously added. You can then correct your error and re-generate from scratch.

## A special sort of table: user accounts

When each row in a table is going to hold a _person_, i.e. it's going to need a column for `email`, `password_digest`, etc; then you probably shouldn't use the `draft:resource` generator. This is because you _do not_ want to generate the standard RCAVs (`index`, `show`, etc) with the standard forms to add a new record, update a record, delete a record, etc.

Instead, you need routes, actions, and templates for signing up, signing in, editing profile, cancelling an account, etc. The RCAVs that the `draft:resource` generator writes for you require so much modification to transform into those pages that it is easier to just start from scratch with the `model` generator.

Fortunately, however, there is another generator that will give you a nice head start towards the boilerplate required for most sign-in/sign-out RCAVs: `draft:account`. Let's say you want a model called `user` with columns `first_name`, `last_name`, and `company_id`:

`rails generate draft:account user first_name:string last_name:string company_id:integer`

You *should not include `email`, `password`, `password_confirmation`, or `password_digest`** in the command. `email` and `password_digest` string columns will automatically be added to the table for you.

Explore the generated code. It added:

 - A `User` model file.
 - A migration which will issue the SQL transaction to create the table and columns in the database once we `rails db:migrate`.
 - Routes, actions, and views for:
    - signing up
    - signing in
    - editing profile
    - signing out
    - deleting account

### @current_user

In addition, `ApplicationController`, the `draft:account` generator wrote a method called `load_current_user`:

```ruby
# app/controllers/application_controller.rb

def load_current_user
  the_id = session.fetch(:user_id)

  @current_user = User.where({ :id => the_id }).at(0)
end
```

You can call this method from within any action to easily define an instance variable called `@current_user` which would use the cookie created by the sign-in/sign-up actions to look up the record in the `User` table that represents whoever is signed in.

Furthermore, it wrote a line:

```ruby
# app/controllers/application_controller.rb

before_action(:load_current_user)
```

This line automatically calls the `load_current_user` method before any action is called! That means that `@current_user` is automatically defined within every action and view template. If someone is signed in, it is a `User`; if not, it is `nil`. From there on out, it's up to you to use `@current_user` to personalize the action/view.

### Forcing sign in

The `draft:account` also writes another method within `ApplicationController`:

```ruby
# app/controllers/application_controller.rb

def force_user_sign_in
  if @current_user == nil
    redirect_to("/user_sign_in", { :notice => "You have to sign in first." })
  end
end
```

This method, if called, will redirect the user to the sign in page if they are not already signed in. This is a very common pattern in applications, and saves us the trouble of having to do a million `if @current_user != nil ...` all over the place before we can use `@current_user` safely (assuming we need a `User`).

To call this method before _every_ action, add this line to `ApplicationController`:

```ruby
# app/controllers/application_controller.rb

# Below load_current_user

before_action(:force_user_sign_in)
```

The generator writes this line, but starts it out commented; uncomment it if you want to opt-in to forcing users to sign in.

However, you need to allow them to visit the sign-in/sign-up pages while being signed-out; to do that, in `UserAuthenticationController`, uncomment the following line:

```ruby
# app/controllers/user_authentication_controller.rb

skip_before_action(:force_user_sign_in, { :only => [:sign_up_form, :create, :sign_in_form, :create_cookie] 
```

Now you're all set; a person will have to sign in before they can visit any other action in the application, and you can confidently use `@current_user` knowing that it will actually be a `User` and not `nil`.

## A better Application Layout file

To generate an application layout file that includes links to Bootstrap, Font Awesome, and some boilerplate markup for a nav bar and alert messages, run the command

```bash
rails g draft:layout
```

It will warn you that it is going to overwrite your existing `app/views/layouts/application.html.erb` -- say yes if you are sure that's okay. Copy out any important stuff if necessary, to be re-pasted back in.

## Making Changes

That's it! You now have a solid starting point to work from.

If you decide, however, to [add a new column](https://chapters.firstdraft.com/chapters/770#adding-or-removing-columns-from-your-table), then you'll have to go in and edit all of the relevant files (forms, processing actions) by hand.

There's no magic to this generator; all it did was automate the tedium of building The Golden Five boilerplate by hand. From here on out, it's up to you to add RCAVs and make the interface match your specifications.
