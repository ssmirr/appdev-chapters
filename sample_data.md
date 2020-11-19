# Creating Sample Data 

You have just created your application and are figuring out where to start. A good practice is to create some sample data early on in the life of your application so you will always have a single command to reset your database with good data. For the building of this seeds file we are going to use the [Photogram-signin](https://github.com/appdev-projects/photogram-signin-fall20/tree/no-rake) project. I recommend opening this project in Gitpod and following along.

To start, open this project and start to create data. There are a few ways we can do this. We can run `bin/server` and start to put in data through the forms. We can also go to visit `/rails/db` within our running application and create some records. Also, in case you forgot we can visit a rails console by typing `rails console` into a terminal and start to enter some records. Let's visit a console and create the first record together. 

```
u = User.new
u.username = "Norman"
u.save
```

That is a bit of typing, so maybe we should go to the running application and fill out the forms. Let's sign up a new user, then go to the photos page and create a new photo. Then create a comment on that photo. Maybe add yourself as a fan. Then go back and make a few more photos. Perfect, now log out and make another user and do the same process again. This process sure is time-consuming. What happens if some corrupt data got into your database, and now you need to delete all the records... 

Luckily for you, we have provided sample data in all of the projects because a project without data is pretty boring and can be hard to debug. Now, how did we do that without going in and making the records one at a time? What we do is create a sample data task to populate the database and are going to show you how to do the same.

## Where do we write our sample data? 

In class, whenever we have needed to get data into our database, we were able to run the command `rails sample_data`, but where is that file located? 

If you were to open a class project and search through all the files, you would find that our `sample_data` lives in the `tasks` folder, which is located within the `lib` folder. The path to that would be `lib/tasks/dev.rake`. Now that we have found where our task should live, let's start building the task that can be called in the termianl. 

### The start 
```
desc "Fill the database tables with some sample data"
task({ :sample_data => :environment}) do
  p "Hello, World!"
end
```

If everything is correct, when we run `rails sample_data` in the terminal, the line "Hello, World!" should have appeared in the terminal. So what does this code do so far?

#### `desc "Fill the database tables with some sample data"`

The [`desc`](https://apidock.com/ruby/v1_9_3_125/Rake/DSL/desc) stands for description, and that is what we are doing with this line, we are describing what we are going to do within this task. 

#### `task({ :sample_data => :environment}) do ... end`

Here we are naming our calling the `task` method and giving it a hash as the argument. The key in the Hash is the name of our Task (as a Symbol) and the value should be `:environment`. Using `:environment` as the value will make the Rails environment load in our task giving us access to all models, gems, and initializers. After we finish our `do` with an `end`, our task is ready to call by inputting `rails sample_data` in a terminal. It isn't much yet, but we are starting to connect the dots. 

### Adding Users

When we start to add data to our task, we will need to know what column is available for each table. In the `user.rb` file, we can see the `User` table schema. It looks like this. 

```
# == Schema Information
#
# Table name: users
#
#  id              :integer          not null, primary key
#  comments_count  :integer
#  likes_count     :integer
#  password_digest :string
#  private         :boolean
#  username        :string
#  created_at      :datetime         not null
#  updated_at      :datetime         not null
#
```
With that information, we can add our first data to the database. 
```
user = User.new
user.username = "Pat"
user.private = true
user.comments_count = 0
user.password = "password"
user.likes_count = 0
user.save
```
If we run `rails sample_data we have just created seeded our first user. Now we can copy and paste that code as many times as we need to create or users. Or we can use a loop.
```
3.times do
  user = User.new
  user.username = "Pat"
  user.private = true
  user.comments_count = 0
  user.password = "password"
  user.likes_count = 0
  users.save
end
```


Wow, we just created three users in no time! It might not be the world's most _interesting_ data, since each user has the same username and is private, but it's a start. 

When we create this data, it might be pretty challenging to determine which `User` belongs to any other record if they all show up with the same `username`. How can we create a `User` with a different `username`?

What if we did this? 
```
names = ["Pat", "Raghu", "Jelani"]
bool = [true, false]

 3.times do |count|
    user = User.new
    user.username = names.at(count)
    user.private = bool.sample
    user.comments_count = 0
    user.password = "password"
    user.likes_count = 0
    user.save
    p count
 end
```
Now we are getting somewhere! Each of our users has a different name, and only some might be private. 

#### An aside: Password
 You might ask why did we use the column of `password` even though our schema is `password_digest`? Not to go too far down the rabbit hole, but the `.password` in the seeds file calls the `.password` method written within rails by the `bcrypt` gem. When this method is called, it will take the value you are trying to save as the password and saves it into your `password_digest` column before encrypting it. 

### How do we know if records are getting into the database?
Well, one way for us to do this is to open a `rails console` and do a `User.all.count`. This command will return a number how many users are now in the database. Do we want to do that everytime we seed our datatbase?

Let's make this easier but adding this line of code. 
```
p "Added #{User.count} Users"
```
Now, when we run our task, we should get a nice message showing how many users are in the database. 

### Adding Follow Requests 
Now that we have a few users, we can create a few follow requests. 

We can start off the same as when we were writing the seeds for our `User`. By taking a look at the schema, we see that a `FollowRequest` has a column of `user_id`. 

To solve that, we can create a variable for all of our every `User`.

```
users = User.all
```
Now we can sample from the `users` array to assign `sender_id` and `recipient_id` within our new `FollowRequest`. 

When we create our `FollowRequst`, we will have to save a `user.id` number in the `sender_id` and `recipient_id` column, but how will we do that? Maybe we can get a random user is to call our `users` variable and do a `.sample`. After we have that single `User` record, we can access the id with `.id`. 
```
users = User.all

10.times do 
  fr = FollowRequest.new
  fr.status = bool.sample
  fr.sender_id = users.sample.id 
  fr.recipient_id = users.sample.id 
  fr.save
end

p "Added #{FollowRequest.count} FollowRequests"
```

This loop above will create 10 `FollowRequests`, but do we want a `User` to be able to create a `FollowRequest` for themselves? What will happen if we have duplicate records?

If either of those questions created a concern, it might be a good time to add in some [validations](https://chapters.firstdraft.com/chapters/845) to make sure we are only allowing data into our database that we want to be there.  With validations will will be able to prevent those records from entering our database.

### Adding the remainder of the data

We can handle the rest of the data in our seeds file similarly to the `FollowRequest` above. It will result in the following code. 

```
10.times do 
  photo = Photo.new
  photo.caption = "This is my photo"
  photo.image = "https://tinyurl.comy6hk6oep"
  photo.likes_count = 1
  photo.owner_id = users.sample.id
  photo.save
end

p "Added #{Photo.count} Photos"

photos = Photo.all 

10.times do 
  like = Like.new
  like.fan_id = users.sample.id 
  like.photo_id = photos.sample.id
  like.save
end

p "Added #{Like.count} Likes"

comments = ["Cool", "I like it", "Love it"]
  
10.times do 
  comment = Comment.new
  comment.body = comments.sample
  comment.author_id = users.sample.id 
  comment.photo_id = photos.sample.id 
  comment.save
end

p "Added #{Comment.count} Comments"
```

Okay, we are almost done!

### Updating our counts for users and photos

What we haven't done yet is corrected the `likes_count` and `comments_count` for each `User` and the `likes_count` for each `Photo`. We can do this with the following code after at the end of our code after the `User`, `Like`, and `Comment` data have been created.

```
User.all.each do |user|
  user.comments_count = Comment.where(:author_id => user.id).count
  user.likes_count = Like.where(:fan_id => user.id).count
  user.save
end

Photo.all.each do |photo|
  photo.likes_count = Like.where(:photo_id => photo.id).count
  photo.save
end
```

Congratulations, with the code you just added, you have fully seeded your database! The problem is that we only have three users. What if we want 50 usernames or 100? Do you think you could create an array with all of those usernames? Is there a faster way to get usernames? 

# Adding interesting data with Faker

Instead of having to come up with all of this data, let's use the [`faker gem`](https://github.com/faker-ruby/faker). What this will do for us is let us pick from a bunch of data sets to sample. To use this in our project, we would install it like any other gem. 

First, add this to our gem file. 
```
gem 'faker', :git => 'https://github.com/faker-ruby/faker.git', :branch => 'master'
```
To install the gem we need to run `bundle install` in a Terminal.

Now in our `sample_data` task, we will have to put `require "faker"` at the top of our seeds file within the task
```
require 'faker'
```
After this, we have all the power to update our `sample_data` file once again. Let's look at what our file might look like after adding the `faker` gem. 

```
25.times do 
  user = User.new
  user.username = Faker::Name.first_name 
  user.private = Faker::Boolean.boolean
  user.password = "password"
  user.comments_count = 0
  user.likes_count = 0
  user.save
end
```
Now each of our records will have a name sampled from the faker dataset of names. We can do this with all of our columns. 

With Faker, we have access to all types of datasets. I highly recommend taking a look to see all of the [different generators](https://github.com/faker-ruby/faker#generators). 

Looking at the faker documentation, we can scroll down to the Generators section.In this section, we can select one of the links. For our seeds file, we selected `Faker::Name`. Inside this link, you can see all of the different methods we can call on `Faker::Name` to return something different. Try some of them in your application!

### Remove old data

Now that we can create all of this data, how do we keep our datasets clean? What if we change our seeds file and want to add more records? What happens to the old data, does it just stay? How do we remove this stale data?

Luckily for us, we can add the following lines to the beginning of our `sample_data` task. 

```
desc "Fill the database tables with some sample data"
task({ :sample_data => :environment}) do

 User.destroy_all
 FollowRequest.destroy_all
 Like.destroy_all
 Photo.destroy_all
 User.destroy_all

end
```

The `.destroy_all` method calls each record and destroys each record and each record associated with it. This will allow us to start with good data everytime we run the `sample_data` task. 

 **Sidenote - `.destroy_all` is a very permanent method and shouldn't be used with production data that belongs to your users**,