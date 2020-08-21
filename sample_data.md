# Creating Sample Data 

You have just created your application and are figuring out where to start.  A good pratice is to create some sample data early on in the life of you application so when a mistake is made, and bad data gets into your database while testing, you will always single command to reset your database with good data. 

### Where do write our sample data? 

In class whenever we have needed to get data into our database we were able to run the command `rails sample_data`, but what where is that file located? 

If you were to open a class project at do a search you woul find that our `sample_data` lives in the `task` folder within the `lib` folder.  The path to that would be `lib/tasks/dev.rake`.  In the beginning of the class whenver we have run a ruby file we would have to call `ruby filename.rb`, so we might think that to run this file we would need to run `ruby dev.rake`, but we are able to change this by how we write out task.  Let's look at the `sample_data` taks for the [photogram gui assignment](https://github.com/appdev-projects/photogram-gui) and break down what is happening so we can build our own `sample_data` task.  

## The code 
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

Ok, sooooo I chopped out four thousand or so lines of code, but this is pretty much it no real magic here and now as seasonsed rubiests this shouldn't look too foreign to you as it is all just ruby.  Lets go through line by line see what is being done. 

#### `desc "Fill the database tables with some dummy data"`

The [`desc`](https://apidock.com/ruby/v1_9_3_125/Rake/DSL/desc) stands for description, and that is what we are doing with this line, we are are describing what we are going to do with this task. 

#### `task({ :sample_data => :environment}) do`

Here we are naming our task and making it available in our coding enviroment.  We can name `sample_data` whatever we would like and now call it with `rails sample_data`.  As long as we finish our `do` with an `end` we have our task, not very useful yet, but it works.  

#### `starting = Time.now`

We are defining a variable called `starting` with will store the current time.  This if so at the end we can put a message which tells us how long our task took to run.  

#### Big block of code 
```
  if Rails.env.production?
    ActiveRecord::Base.connection.tables.each do |t|
      ActiveRecord::Base.connection.reset_pk_sequence!(t)
    end
  end
  ```
  Ok so this one is more than one line but not super difficult to explain.  What this block of code does it checks to see if we are in a production enviroment.  If we go to a rails console and type in `Rails.env.prouction` it will return false, because currently we are in a `development` envrioment, but when we push to heroku we will then be in our `production` enviroment.  So if you would like you could take away the line of `Rails.env.production?`, you can do that and it would run the code inside that logic wherever you run your task.  

  I will just tell you what the rest of the code does and you can feel free to use it in your file.  If you were to go to a console and type in `ActiveRecord::Base.connection.tables` it would return an array of our current tables in our project.  Now just as we have done before we are calling `.each` on that array and now making a connection to our database with `ActiveRecord::Base.connection`, but why are we calling the `.reset_pk_sequence!(t)`.  We know that `t` is the name of our table so what is `reset_pk_sequence!`? Well has this happened to you yet when you are signed in as a user and create a record and put the foreign `user_id` key as the `id` of the user you are on.  Now let's say you delete that user but recreate them, they will have a different `id`.  You will never see two records in the same class with the same id's and you will never see an `id` being reused.  So if I am going to create sample data and put the `user_id` as `1`, then if I were to run my `sample_data` again and delete all of my users, that `user_id` I assigned as `1` wouldn't exist because we don't reuse `id` numbers. 

  How we fix that is with the `.reset_pk_sequence!` command.  This will reset the `id` numbers of the database and start again at 1. 


####  `User.delete_all`
What we are doing a few times here is going into our database and deleting all of the records we currently have.  We can call this on any of the classes we have created. 

#### `users = [{id: 81, username: "galen", private: false, likes_count: 97, comments_count: 98, created_at: "2015-01-19 09:24:34", updated_at: "2019-10-08 10:25:00"},`

This is something we have seen a few times this quarter.  We are creating the variable `users` which is an array and inside the array we are filling it with a hashes that contain the information we want to save for each record.  We actually can leave out the `created_at` and `updated_at`, these will be created when the records are saved. 

#### `Like.insert_all!(likes)`

This is at the end of our but we would do this after each array.  For our `users` we would call `User.insert_all!(users)`.  The `.insert_all` will take all of the data in our `users` array and insert it all in the database.  

#### Deteming elapsed time
``` 
ending = Time.now
elapsed = ending - starting
```
Here we define our ending time and set a variable of `elapsed` which is the time it took to run our `sample_data`

#### A nice little message
```
puts "#{elapsed.to_i} seconds elapsed." puts "Generated Dummy Data"
  ```
Now we just end our task with a nice little message which tells us how long it took to run our task and that it was completed.  

### Wrapping up

Perfect, you are all set and ready to go, just start writting your four thousands lines of sample data. 

But wait!!! What if we don't want to write four thousand lines to get some sample data in our database, is there anything we can do?!?!  
... <br>
..... <br>
...... <br>
........ <br>
Of course there is!! This... is... RAILSSSSS!

# Refactoring `sample_data` 
Just like our past assignments were able to be refactored, this `sample_data` is no different.  

The first few lines will be the same but we will start to refactor as soon as we start creating our data

### Starting with Users

In the example above we created users by manually inputing data from an array and isn't an array a collection of items, well if we know columns of the tables are we can easily fill these out.  

Becuase we are dealing with ruby we can use all of the ruby we know so what if we did something like this 
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

Wow, we just created 100 users in no time!! I mean yes it might be the worlds most boring data, but who wouldn't want 100 users named "Pat"? 

OK, I understand, how can we make this better? 

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
Now we are getting somewhere, and our 100 users will come from 4 seperate names, isn't this so much better? 

Now we could this for each of our tables and create arrays of whatever we want and sample the data for each of the records, but wouldn't it be better if there was a giant collection of records we could sample from? Yes, yes it would and again because this is Rails, let's get help from the community and not reinvent the wheel.  

# Refactoring with Faker

Now instead of us having to come up with all of these datasets lets use the [`faker gem`](https://github.com/faker-ruby/faker).  What this will do for us is let us pick from a bunch of data sets to sample from.  How we use this in our project we would install it like any other gem. 

First, add this to our gem file. 
```
gem 'faker', :git => 'https://github.com/faker-ruby/faker.git', :branch => 'master'
```
Then we need to run our `bundle install`

Now in our `sample_data` file we will have to `require faker` with 
```
require 'faker'
```
After this we have all the power to refactor our `sample_data` file once again. Let's take a look at what our file might look like after adding the `faker gem`. 

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
Now each of our records will have name sampled from the faker database of names.  We can do this with all of our columns. Also we can refactor this again and change the code again. 
```
100.times do 
    user = User.new
    user.username = Faker::Name.first_name 
    user.private = Faker::Boolean.boolean
    user.save
end
```
All we did here was change doing a mass input to inserting the records indivdually. 

With faker we have access to all types of datasets, I highly recommend taking a look to see all of the [different generators](https://github.com/faker-ruby/faker#generators). 

### Seeding foreign keys

The only thing left for us would be to sample `user_id` into one of our other tables. This isn't to tricky but if you are wondering we can always call `Class.all.sample`. An example below would be if I wanted food itmes to belong to a user. 

```
1000.times do 
    food = Food.new
    food.name = Faker::Food.dish
    food.description = Faker::Food.description
    food.user_id = User.all.sample.id
    food.save
end
```
Now the random foods I created will belong
to one of my users.  

Be creative in creating your seeds file, try all different types of data and remember it is only ruby.  




