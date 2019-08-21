# Signing In Users

## Create a table

Almost always, we call this model `User`. You could call it `Teacher` or `Driver` or whatever, but other developers will instantly understand what each record in this this table represents — a user who will be signing in and out — if you call it `User`.

At a minimum, you must include two columns in this table:

 - A way to uniquely identify each user. This almost always `email` (so that you have a way to reach them in case they forget their password).
 - A way to prove who the user is. This is almost always a password that they choose. Since we don't ever want to store passwords in our database, we instead store a scrambled-up version of their password so that even we don't know what it is; we call this a "digest", and we'll call the column `password_digest`.
 - You can also include any other columns that you want in the table — `first_name`, `last_name`, `username`, etc.

For example:

```
rails generate draft:model user email:string password_digest:string username:string
```

## Build the sign-up endpoint

Users are ultimately just records in a table, like directors or movies. So we need a route which will trigger the insertion of a row, as usual.

```ruby
# config/routes.rb

match("/insert_user", { :controller => "users", :action => "create", :via => "post" })
```

Let's add the controller:

```ruby
class UsersController < ApplicationController
end
```

And in the action, we'll assume that there will be keys called `"email"`, `"password"`, `"first_name"`, etc, in the `params` hash.

```
def create
  
end
```


## Build the sign-in endpoint
