# Creating Sample Data 

You have just created your application and are figuring out where to start. A good practice is to create some sample data early on in the life of your application so you will always have a single command to reset your database with good data. For the building of this seeds file we are going to use the [Photogram-signin](github.com/appdev-projects/photogram-signin-fall20) project.  I recommend opening this project in Gitpod and following along.  

To start, open this project and start to create data.  First, create a user, then go to the photos page and create a new photo.  Then create a comment on that photo.  Maybe add yourself as a fan.  Then go back and make a few more photos.  Perfect, now log out and make another user and do the same process again.  This process sure is time-consuming.  What happens if some corrupt data got into your database, and now you need to delete all the records... 

Luckily for you, we have provided sample data in all of the projects because a project without data is pretty boring and can be hard to debug.  Now, how did we do that without going in and making the records one at a time? What we do is create a sample data task to populate the database and are going to show you how to do the same.

## Where do we write our sample data? 

In class, whenever we have needed to get data into our database, we were able to run the command `rails sample_data`, but where is that file located? 

If you were to open a class project and do a search, you would find that our `sample_data` lives in the `tasks` folder, which is located within the `lib` folder. The path to that would be `lib/tasks/dev.rake`. When we first started learning, whenever we want to run a Ruby file, we would have to call `ruby filename.rb`, so we might think that we would need to run `ruby dev.rake` to run this file. While this would follow our previous convention, we are able to define how we call these tasks in a rake file. Let's start building the task by being able to call it in the rails terminal.

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

#### `task({ :sample_data => :environment}) do`

Here we are naming our task and making it available in our coding environment.  By defining our task `sample_data`, it is ready for us to call with `rails sample_data`. As long as we finish our `do` with an `end`, we have our task. It isn't much yet, but we are starting to connect the dots. 

### Remove old data

Now that we can access our task, the first thing we should do is clear out any old data.  Based on our project schema, I know that we have the tables for `Comments`, `Follow_requests`, `Likes`, `Photos`, and `Users`.  

I will add the following lines to our `sample_data` task. 

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

The `.destroy_all` method calls each record and destroys each record and each record associated with it. Now that we have a clean database, we can start adding records to our database. 

### Adding Users

When we start to add data to our task, we will need to know what column is available for each table.  In the `user.rb` file, we can see the `users` table schema. It looks like this. 

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

Wow, we just created three users in no time!! I mean, yes, it might be the world's most boring data, but who wouldn't all of their users with the same name? 

Okay, so how can we make this better? 

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
Now we are getting somewhere, each of our users has a different name, and some might be private. 

Sidenote: Why did we use the column of `password` even though our schema is `password_digest`?  Not to go too far down the rabbit hole, but the `.password` in the seeds file calls the `.password` method written within rails by the `bcrypt` gem.  When this method is called, it will take the value you are trying to save as the password and saves it into your `password_digest` column before encrypting it. 

### Adding Follow Requests 
Now that we have a few users, we can create a few follow requests. 

We can start off the same as when we were writing the seeds for our `User`.  By taking a look at the schema, we see that a `FollowRequest` takes a `user.id`. 

To solve that, we can create a variable for all of our `Users`

```
users = User.all
trueorfalse = [true, flase]
```
This will let us to sample from this array within our new `FollowRequest`. Our loop will look like this. 

```
users = User.all
truefalse = [true, false]

10.times do 
  fr = FollowRequest.new
  fr.status = bool.sample
  fr.sender_id = users.sample.id 
  fr.recipient_id = users.sample.id 
  fr.save  
end

p "Added #{FollowRequest.count} FollowRequests"
```

This loop above will create 10 `FollowRequests`. Now there is no way to tell if they are duplicates or not, so it might be a good time to add in some [validations](https://chapters.firstdraft.com/chapters/845) to make sure we are only allowing data into our database that we want to be there.

### Adding the remainder of the data

We can handle the rest of the data in our seeds file similarly to the `FollowRequest` above. It will result in the following code. 

```
10.times do 
    photo = Photo.new
    photo.caption = "This is my photo"
    photo.image = "https://tinyurl.com/y6hk6oep"
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

Okay, we are almost done...

### Updating our counts for users and photos

What we haven't done yet is corrected the `likes_count` and `comments_count` for `Users` and the `likes_count` for `Photos`.  We can do this with the following code.

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

Congratulations, with the code you just added, you have fully seeded your database.  Now, what if we want some data that is a little bit more interesting. 

# Adding interesting data with Faker

Now, instead of having to come up with all of these data, let's use the [`faker gem`](https://github.com/faker-ruby/faker). What this will do for us is let us pick from a bunch of data sets to sample. To use this in our project, we would install it like any other gem. 

First, add this to our gem file. 
```
gem 'faker', :git => 'https://github.com/faker-ruby/faker.git', :branch => 'master'
```
Then we need to run our `bundle install`

Now in our `sample_data` file, we will have to put `require faker` at the top of our seeds file within the task
```
require 'faker'
```
After this, we have all the power to update our `sample_data` file once again. Let's look at what our file might look like after adding the `faker gem`. 

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
Now each of our records will have a name sampled from the faker database of names.  We can do this with all of our columns. 

With Faker, we have access to all types of datasets. I highly recommend taking a look to see all of the [different generators](https://github.com/faker-ruby/faker#generators). 







