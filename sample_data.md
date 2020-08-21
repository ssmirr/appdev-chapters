# Creating Sample Data 

You have just created your application and are figuring out where to start. A good practice is to create some sample data early on in the life of your application so you will always have a single command to reset your database with good data. 

## Where do we write our sample data? 

In class, whenever we have needed to get data into our database, we were able to run the command `rails sample_data`, but what where is that file located? 

If you were to open a class project and do a search you would find that our `sample_data` lives in the `task` folder, which is located within the `lib` folder. The path to that would be `lib/tasks/dev.rake`. In the beginning of the class, whenver we have run a Ruby file we would have to call `ruby filename.rb`, so we might think that to run this file we would need to run `ruby dev.rake`. While this would follow our previous convention, we are able to define how we call these tasks. Let's look at the `sample_data` task for the [photogram gui assignment](https://github.com/appdev-projects/photogram-gui) and break down what is happening so we can build our own `sample_data` task.  

### The code 
```
desc "Fill the database tables with some dummy data"
task({ :sample_data => :environment}) do
  starting = Time.now

  if Rails.env.production?
    ActiveRecord::Base.connection.tables.each do |t|
      ActiveRecord::Base.connection.reset_pk_sequence!(t)
    end
  end

  User.delete_all
  Photo.delete_all
  Like.delete_all
  Comment.delete_all
  FollowRequest.delete_all

  users = [
    {id: 81, username: "galen", private: false, likes_count: 97, comments_count: 98, created_at: "2015-01-19 09:24:34", updated_at: "2019-10-08 10:25:00"},
    {id: 82, username: "trina", private: false, likes_count: 22, comments_count: 35, created_at: "2014-09-02 06:05:56", updated_at: "2019-10-08 10:25:00"},...

(FOUR THOUSAND LINES OF CODE!!!!)

  Like.insert_all!(likes)
  ending = Time.now
  elapsed = ending - starting

  puts "#{elapsed.to_i} seconds elapsed."
  puts "Generated Dummy Data"
end
```

Okay, sooooo I chopped out four thousand or so lines of code, but as you can see, there no real magic here, and as seasoned Rubyists, we should be somewhat familiar with what is happening.  Still, let's go through line by line and see what is being done. 

#### `desc "Fill the database tables with some dummy data"`

The [`desc`](https://apidock.com/ruby/v1_9_3_125/Rake/DSL/desc) stands for description, and that is what we are doing with this line, we are describing what we are going to do within this task. 

#### `task({ :sample_data => :environment}) do`

Here we are naming our task and making it available in our coding environment.  By defing our task `sample_data`, it is ready of us to call with `rails sample_data`. As long as we finish our `do` with an `end`, we have our task, not very useful yet, but it works.  

#### `starting = Time.now`

We are defining a variable called `starting`, which will store the current time.  Now at the end of our program, we can put a message which tells us how long our task took to run. 

#### Big block of code 
```
  if Rails.env.production?
    ActiveRecord::Base.connection.tables.each do |t|
      ActiveRecord::Base.connection.reset_pk_sequence!(t)
    end
  end
  ```
  Okay, so this one is more than one line but not super difficult to explain. What this block of code does it checks to see if we are in a production environment.  If we go to a rails console and type in `Rails.env.prouction` it will return `false`, because currently, we are in a `development` environment, but when we push to Heroku, we will then be in our `production` environment. So if you would like, you could take away the line of `if Rails.env.production?` and the matching `end`. If you do, the following logic will run wherever you run your task.   

I will tell you what the rest of the code does, and you can feel free to use it in your file. If you were to go to a console and type in `ActiveRecord::Base.connection.tables`, it would return an array of the current tables in our project.  Now, just as we have done before, we are calling `.each` on that array. Making a connection to our database with `ActiveRecord::Base.connection`, we are calling the method `.reset_pk_sequence!(t)`. We know that `t` is the name of our table, so what is `reset_pk_sequence!`? Well, has it happened yet that the foreign `user_id` has changed after deleting a user and adding that user back? What is happening is two records in the same class will never have the same id's. Even if a record is deleted, that deleted `id` will not be used again. So, if you create sample data and put the `user_id` as `1`, then run `sample_data` again and delete all of the users, that `user_id`, which was assigned as `1` wouldn't exist because we don't reuse `id` numbers.  

How we fix this is with the `.reset_pk_sequence!` command. It will reset the `id` numbers of within our database and start again at 1. 


#### `User.delete_all`
What we are doing a few times here is going into our database and deleting all of the records we currently have. We can call this on any of the classes we have created. 

#### `users = [{id: 81, username: "galen", private: false, likes_count: 97, comments_count: 98, created_at: "2015-01-19 09:24:34", updated_at: "2019-10-08 10:25:00"},`

This code is something we have seen a few times this quarter. We are creating the variable `users`, which is an array. Inside the array, we fill it with hashes containing the information we want to save for each `user` record. We actually can leave out the `created_at` and `updated_at`, they are created when the records are saved.  

#### `Like.insert_all!(likes)`

Yes, this is for `Like` but we would do this after each array.  For our `users` we would call `User.insert_all!(users)`. The `.insert_all` will take all of the data in our `users` array and insert it in the database.  

#### Deteming elapsed time
``` 
ending = Time.now
elapsed = ending - starting
```
Here we define our ending time and set the variable of `ending`, this is used is with our `starting` variable from the top to determine the `elapsed` time it took to run `sample_data`

#### A nice little message
```
puts "#{elapsed.to_i} seconds elapsed." 
puts "Generated Dummy Data"
  ```
At the end of our task, we give ourselves a nice little message which tells us how long it took to run our task and that it is complete.  

### Wrapping up

Perfect, you are all set and ready to go; just start writing your four thousand lines of sample data. 

But wait!!! What if we don't want to write four thousand lines to get some sample data in our database, is there anything we can do?!?!  

... <br>
..... <br>
...... <br>
........ <br>

Of course there is!! This... is... RAILSSSSS!

# Refactoring `sample_data` 
Just like our past assignments were able to be refactored, this `sample_data` is no different.  

The first few lines will be the same, but we will start to refactor as soon as we start creating our data.

## Starting with Users

In the example above we created users by manually inputting data from an array and isn't an array a collection of items, well if we know columns of the tables are we can easily fill these out.  

Because we are dealing with Ruby, we can use all of the Ruby we know so what if we did something like this 
```
users = []
100.times do 
    user = User.new
    user.username = "Pat"
    user.private = true
    users.push(user)
end
Users.insert_all!(users)
```

Wow, we just created 100 users in no time!! I mean yes, it might be the world's most boring data, but who wouldn't want 100 users named "Pat"?  

Okay, I understand, how can we make this better? 

What if we did this? 
```
names = ["Pat", "Raghu", "Jelani", Shreya"]
bool = [true, false]
users = []
100.times do 
    user = User.new
    user.username = names.sample
    user.private = bool.sample 
    users.push(user)
end
Users.insert_all!(users)
```
Now we are getting somewhere, and our 100 users will come from 4 different names, this is better. 

We could this for each of our tables, and create arrays of whatever we want and sample the data for each of the records, but wouldn't it be better if there was a giant collection of records that we could sample? Yes, yes it would and again because this is Rails, let's get help from the community and not reinvent the wheel.    

# Refactoring with Faker

Now, instead of having to come up with all of these datasets, let's use the [`faker gem`](https://github.com/faker-ruby/faker). What this will do for us is let us pick from a bunch of data sets to sample. To use this in our project, we would install it like any other gem. 

First, add this to our gem file. 
```
gem 'faker', :git => 'https://github.com/faker-ruby/faker.git', :branch => 'master'
```
Then we need to run our `bundle install`

Now in our `sample_data` file, we will have to `require faker` with 
```
require 'faker'
```
After this, we have all the power to refactor our `sample_data` file once again. Let's take a look at what our file might look like after adding the `faker gem`. 

```
users = []
100.times do 
    user = User.new
    user.username = Faker::Name.first_name 
    user.private = Faker::Boolean.boolean
    users.push(user)
end
Users.insert_all!(users)
```
Now each of our records will have a name sampled from the faker database of names.  We can do this with all of our columns. Also, we can refactor this again.
```
100.times do 
    user = User.new
    user.username = Faker::Name.first_name 
    user.private = Faker::Boolean.boolean
    user.save
end
```
Here we changed from doing a mass input to inserting the records individually. 

With faker, we have access to all types of datasets. I highly recommend taking a look to see all of the [different generators](https://github.com/faker-ruby/faker#generators). 

## Seeding foreign keys

The only thing left for us would be to sample `user_id` into one of our other tables. This isn't too tricky because we can always call `Class.all.sample`. An example below would be if I wanted food items to belong to a user.  

```
1000.times do 
    food = Food.new
    food.name = Faker::Food.dish
    food.description = Faker::Food.description
    food.user_id = User.all.sample.id
    food.save
end
```
Now the random foods I created will belong to one of my users.  

Be creative in creating your seeds file; try all different types of data, and remember it is only Ruby.  




