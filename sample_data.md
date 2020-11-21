# Creating Sample Data 

## Getting Started!

You have just created your application and are figuring out where to start. A good practice is to create some sample data early on in the life of your application so you will always have a single command to reset your database with good data. Together we are going to build a `sample_data` task using the [Photogram-signin](https://github.com/appdev-projects/photogram-signin-fall20/tree/no-rake) project. I recommend opening this project in Gitpod and following along.

After the repo has opened in Gitpod start to create some data. There are a few ways we can do this. We can run `bin/server` and start to put in data through the forms. We can visit `/rails/db` within our running application and create some records. Lastly, we can open a rails console by typing `rails console` into a terminal and start to enter some records.  If you are in a rails console, here is the code to create your first `User` record.

```
u = User.new
u.username = "Norman"
u.save
```

That is a bit of typing, so maybe we should go to the running application and fill out the forms. Let's sign up a new user, then go to `/photos` and create a new photo. We can then click on the details of a photo, and create a comment, make sure to add the correct `author_id`. Then go back `/photos` and make a few more `Photo` records, adding comments each time. 

This process sure is time-consuming! What if some invalid data gets into your database and breaks your app? Now you need to delete all the records and start making records one at a time again...  

Luckily for you, we have provided sample data in all of the projects because a project without data is pretty boring and can be hard to debug. Now, how did we do that without going in and making the records one at a time? What we did was create a `sample_data` task to populate the database, and we are going to show you how to create your own!

## Where do we write our sample data task? 

In class, whenever we have needed to get data into our database, we were able to run the command `rails sample_data`, but where is that command is defined? 

If you were to open a class project and search through all the files, you would find that our `sample_data` lives in the `tasks` folder, which is located within the `lib` folder. The path to that would be `lib/tasks/dev.rake`. But, what is a task?

A "task" is just a small program that does useful things in our application like backing up the database, generating reports, or sending out email notices to your users. With everything in Rails, there is a designated location for the files that define the tasks for our application, `lib/tasks/`.

Now that we know where our `sample_data` task should live, let's start building a task that can be called in the terminal. 

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

Here we are naming our calling the `task` method and giving it a hash as the argument. The key in the Hash is the name of our task (as a Symbol) and the value should be `:environment`. Using `:environment` as the value will make the Rails environment load in our task giving us access to all models, gems, and initializers. After we finish our `do` with an `end`, our task is ready to call by inputting `rails sample_data` in a terminal. It isn't much yet, but we are starting to connect the dots. 

### Adding Users

When we start to add data to our `sample_data` task, we will need to know what columns are available for each table. In the `user.rb` file, we can see the `User` table schema. It looks like this. 

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

By calling the column names on a new instance of the `User` table, we can now assign values to the columns of our new `User` record. 

```

user = User.new
user.username = "Pat"
user.private = true
user.comments_count = 0
user.likes_count = 0
user.save

```
If we run `rails sample_data` we have just created our first user. Now we can copy and paste that code as many times as we need to create more users. Or we can use a loop.
```

3.times do
  user = User.new
  user.username = "Pat"
  user.private = true
  user.comments_count = 0
  user.likes_count = 0
  users.save
end
```

Wow, we just created three users in no time! It might not be the world's most _interesting_ data, since each user has the same username and is private, but it's a start. 

When we create this data, it might be pretty challenging to determine which `User` belongs to any other record if they all show up with the same `username`. How can we create our code so that each `User` record could have a different `username`?

What if we did this? 

```
names = ["Pat", "Raghu", "Jelani"]
bool = [true, false]

 3.times do |count|
    user = User.new
    user.username = names.at(count)
    user.private = bool.sample
    user.comments_count = 0
    user.likes_count = 0
    user.save
 end
```

Now we are getting somewhere! By using `names.at(count)` within our [loop](https://chapters.firstdraft.com/chapters/764), we will be able to access a different name from our `names` array and assign it to our `username` column.  For the user's `private` column, we can call the `.sample` on our `bool` array; This will set a user's `private` column to either `true` or `false`.

#### An aside: Passwords
What happens if you need to add a password? If you were to run the firstdraft draft generators the generator would add the column of `password_digest` to the schema.  Once this is added you would think you would want to add call `.password`[^1] column to the `user`. The code will look like this.

```
  user.password = "my_password"
```

### How do we know if records are getting into the database?
If we have a server running we can visit `/rails/db` to see if there are any records. Another way is to open a `rails console` and do a `User.all.count`. This command will return a number how many users are now in the database. Do we want to do that everytime we create a record in our datatbase?

Let's make this easier but adding this line of code. 

```
p "Added #{User.count} Users"
```

Now, when we run our task, we should get a nice message showing how many users are in the database. 

### Adding Follow Requests 
Now that we have a few users, we can create a few follow requests. 

We can start off the same as when we were creating records for our `User` by taking a look at the schema.

```
# == Schema Information
#
# Table name: follow_requests
#
#  id           :integer          not null, primary key
#  status       :string
#  created_at   :datetime         not null
#  updated_at   :datetime         not null
#  recipient_id :integer
#  sender_id    :integer
#
```

It appears that a `FollowRequest` has a column of `sender_id` and `recipient_id`.  So how are we going to get a `User` record from our database and get the `id` number from that record for our new `FollowRequest`? 

One way to do it is to create a variable that contains all the `User` records.

```
users = User.all
```

By creating the variable of `users`, we can call the `.sample` method to retrieve one random `User` record. Once we have that record, we can then call the `.id` method to extract the id number from that `User`.  This user's id number can then be assigned to the `sender_id` and `recipient_id` column for our new `FollowRequest` record. See the code below for an example.

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

This loop above will create 10 `FollowRequests` where the sender and receiver are random existing users. 

A few things to consider:
 - What would happen if the random user is the sender _and_ receiver of a FollowRequest?
 - Do we want a `User` to be able to create a `FollowRequest` for themselves?
 - If the sender and receiver are random, what will happen if duplicate records are created? Should we allow them?

If any of these points created a concern, it might be a good time to add in some [validations](https://chapters.firstdraft.com/chapters/845) to make sure we are only allowing data into our database that we want to be there.  With validations, we can define rules for which data is allowed to enter our database.

### Adding the remainder of the data

We can handle the rest of the data in our task similarly to the `FollowRequest` above. It will result in the following code. 

```
photos = ["https://tinyurl.comy6hk6oep", https://tinyurl.com/y5uszprj, https://picsum.photos/200]
10.times do 
  photo = Photo.new
  photo.caption = "This is my photo"
  photo.image = photos.sample
  photo.likes_count = 0
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

Congratulations, with the code you just added, you have fully created your database! The problem is that we only have three usernames. What if we want 50 usernames or 100? Do you think you could create an array with all of those usernames? Is there an easier way to get usernames? 

# Adding interesting data with Faker

Instead of having to come up with all of this data, let's use the [`faker gem`](https://github.com/faker-ruby/faker). This will let us sample data from a bunch of data sets. To add this to our project, we can install it like any other gem.

First, open `Gemfile` and add the following line. 

```
gem 'faker', :git => 'https://github.com/faker-ruby/faker.git', :branch => 'master'
```

To install the gem we need to run `bundle install` in a Terminal.

Now in our `sample_data` task, we will have to put `require 'faker'` at the top of our task after the task name. 

```
require 'faker'
```

After this, we have the power to update our `sample_data` file once again. Let's look at what our file might look like after adding the `faker` gem. 

```
25.times do 
  user = User.new
  user.username = Faker::Name.first_name 
  user.private = Faker::Boolean.boolean
  user.password = "password"
  user.save
end
```

Now each of our records will have a name sampled from the Faker dataset of names. Are there any other columns in our app that would benefit from Fakers' datasets?

With Faker, we have access to all types of datasets. I highly recommend taking a look to see all of the [different generators](https://github.com/faker-ruby/faker#generators). 

Looking at the faker documentation, we can scroll down to the Generators section. In this section, we can select one of the links. For our `sample_data` task, we selected `Faker::Name`. Inside this [link](https://github.com/faker-ruby/faker/blob/master/doc/default/name.md), you can see all of the different methods we can call on `Faker::Name` to sample a different dataset. Try some of them in your application!

### Remove old data

Now that we can create all of this data, how do we keep our datasets clean? What if we change our `sample_data` task and want to add more records? What happens to the old data, does it just stay? How do we remove this stale data?

Luckily for us, we can add the following lines to the beginning of our `sample_data` task. 

```
desc "Fill the database tables with some sample data"
task({ :sample_data => :environment}) do

 User.destroy_all
 FollowRequest.destroy_all
 Like.destroy_all
 Photo.destroy_all
 User.destroy_all

 ...

end
```

The `.destroy_all`[^2] method calls each record and destroys each record and each record associated with it. This will allow us to start with good data everytime we run the `sample_data` task. 

[^2]:**`.destroy_all` is a very permanent method and shouldn't be used with production data that belongs to your users**

[^1]Why did we use the column of `password` even though our schema is `password_digest`? Not to go too far down the rabbit hole, but the `.password` in the code above calls the `.password` method written within rails by the `bcrypt` gem. When this method is called, it will take the value you are trying to save as the password and saves it into your `password_digest` column before encrypting it. 
