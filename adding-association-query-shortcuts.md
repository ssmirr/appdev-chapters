# Adding Association Query Shortcuts

In this project, we're going to practice querying our tables more; in particular, we want to practice leveraging the foreign key columns we added to traverse **associations** between tables.

And, since we make these particular queries (the ones to find associated rows from other tables) so often, we're going to add instance methods that wrap up the logic and make it a snap to use.

## Setup

 1. In Codio, find the `photogram-queries` workspace.
 1. 🚀 Initial project setup
 1. 🚀 Start web server
 1. ▶️ Live app — You should see "Yay! You're on Rails" if all went well.

## The domain model

Here's the what our domain model looks like in this appliction in [Entity Relationship Diagram](https://www.lucidchart.com/pages/er-diagrams#discovery__top){:target="_blank"} notation:

![](/assets/photogram-queries-erd.png)

It's similar to the paper database we printed out on Day 1; if you have it, pull it out, because it will be very helpful to look at it while you are planning out how to write your queries.

I've already created these tables by running the following commands:

```bash
rails generate draft:model user username:string private:boolean likes_count:integer comments_count:integer
```

```bash
rails generate draft:model photo caption:text image:string owner_id:integer likes_count:integer comments_count:integer
```

```bash
rails generate draft:model follow_request sender_id:integer recipient_id:integer status:string
```

```bash
rails generate draft:model like fan_id:integer photo_id:integer
```

```bash
rails generate draft:model comment photo_id:integer body:text author_id:integer
```

The `bin/setup` script includes a line that `rails db:migrate`s, so the tables should be all set up.

## Play around in rails console

Pop open a `rails console` and verify that the models got set up correctly:

```ruby
User.count
Photo.count
FollowRequest.count
Like.count
Comment.count
```

As usual, we could create some data manually with `.new`, `.save`, etc; but, to save time, I've included a script for you that will add some dummy data to play around with. At a Terminal prompt, run:

```bash
rails dev:prime
```

or, equivalently, click "🚀 Hydrate with dummy data" in the menu. This will take a few minutes and create 40 users, some photos for each user, some followers, likes, comments, etc.

In your live app, you can visit the URL `http://[YOUR DOMAIN]/admin` (or click "▶️ ActiveAdmin" in the menu) and check out the data being created via our quick-and-dirty dashboard (provided by the [ActiveAdmin gem](https://github.com/activeadmin/activeadmin){:target="_blank"}).

## Resetting the database

If for some reason later you want to reset the database to square one, you need to first destroy it:

```bash
rails db:drop
```

and then re-create it:

```bash
rails db:migrate
```

and then re-populate it:

```bash
rails dev:prime
```

## Queries to write

Your goal, ultimately, is to define the following methods. While you're trying to figure out how to do so, it will probably be helpful to bounce between the `rails console` (to experiment) and your model files (to write multi-step queries).

**Note:** In the Ruby community, a shorthand for saying "an instance method called `zebra` on `Photo`" is `Photo#zebra`. (I know, unfortunately this is yet _another_ thing that the octothorpe symbol is used for.) A shorthand for saying "a **class** method on `Photo` called `zebra`" is `Photo.zebra`.

### Appetizier queries

Here are some `rails console` appetizers to try. For the user `"Trina"`,

  - How many photos has the user posted?
  - How many photos has the user liked?
  - How many comments has the user made?
     - How many photos has the user commented on? Is this the same as the number of comments the user has made? Hint: there's a method called `.distinct` that you can call on a collection to remove duplicates.
  - How many follow requests has the user sent?
     - How many of those were accepted? (I.e. how many people is the user following?)
  - How many follow requests has the user received?
     - How many of those were accepted? (I.e. how many followers does the user have?)
  - What are the usernames of all of the people the user is following?
  - How many photos are in the user's feed (photos posted by the people the user is following)?
  - How many photos have the people the user is following liked?

### Your tasks

Ultimately, define the following instance methods to encapsulate the logic you discovered in the appetizer queries:

 - `Photo#poster` should return the user who posted the photo.
 - `Photo#comments` should return the comments made on the photo.
 - `Comment#commenter` should return the user who authored the comment.
 - `Photo#likes` should return the likes made on the photo.
 - `Photo#fans` should return the users that have liked the photo.
 - `Photo#fan_list` should return the usernames of the users that have liked the photo as a sentence.

    Hint: Rails adds a handy method to `Array`s of strings called  `.to_sentence`:

    https://api.rubyonrails.org/classes/Array.html#method-i-to_sentence
 - `User#comments` should return the comments the user has made.
 - `User#own_photos` should return the photos posted by the user.
 - `User#likes` should return the likes created by the user.
 - `User#liked_photos` should return the photos the user has liked.
 - `User#commented_photos` should return the photos the user has commented on.
 - `User#sent_follow_requests` should return all of the follow requests that were sent by the user.
 - `User#received_follow_requests` should return all of the follow requests that were received by the user.
 - `User#accepted_sent_follow_requests` should return the follow requests that were sent by the user and accepted.
 - `User#accepted_received_follow_requests` should return the follow requests that were received by the user and accepted.
 - `User#followers` should return the people whose follow requests the user has accepted.
 - `User#following` should return the people that have accepted the user's follow requests.
 - `User#feed` should return the photos posted by the people the user is following.
 - `User#discover` should return the photos that are liked by the people the user is following.

[Run `rails grade`](https://chapters.firstdraft.com/chapters/777) when you're ready to see how you're doing.

