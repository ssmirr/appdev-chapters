# CheckIn Alert — Project Notes

## Target

[https://checkin-alert.matchthetarget.com/](https://checkin-alert.matchthetarget.com/)

## Tasks

### Explore the target

 - Sign up as a new user. You can make up a fictional email address and phone number.
 - Our goal, ultimately, will be to write a program that will send a reminder to check in to **any flight that is departing 24 hours and 15 minutes from now, or sooner — unless we've already sent a reminder for that flight.**
 - Add a few flights. Make some that are in the past and some that are in the future. Make sure that at least one is within the reminder window — say, 24 hours and 10 minutes from now. Make another one just outside the reminder window — say, 24 hours and 20 minutes from now.
 - After everyone has created some dummy flights, I will demonstrate running the reminder program in the background.

### Domain model / basic scaffolding

 - Figure out the entire database structure: all tables and all columns. What associations are there? What foreign key columns and/or join tables? For each column, what validation rules, if any?
 - Make an entity relationship diagram in [firstdraft Ideas](https://ideas.firstdraft.com/).
 - Then, create a brand new Rails application. Click "Show Tutorial" at the bottom of your ERD for some help.
 - Create the standard CRUD infrastructure for flights, and the standard sign-up/sign-in/sign-out infrastructure for users.
 - The target has some Bootstrap styling. Later on, if you want to, feel free to add it; for now, I recommend just focusing on functionality. Maybe just use a `<table border="1">` to make things easier to see for now.

### Improve the interface

#### Nav

 - Sign in/Sign up when no one is signed in
 - Edit profile/Sign out when someone is signed in

#### Add a new flight form

 - Get rid of user ID input; it should use the signed in user

#### List of flights

 - The signed in user should see only their own flights, not everyone's flights.
 - Format the copy for each flight.
 - Format the time using the `.strftime` method. The method is called on any `Time`, `Date`, or `DateTime` object and requires a `String` argument that determines how to format the time. For example,

   ```ruby
   photo.created_at.strftime("%a %b %Y")
   ```
   
    would produce something like "Fri Nov 2019". The time formatting codes are a pain to remember, so there are many handy tools to help compose them. I like this one: [http://strftime.net/](http://strftime.net/){:target="_blank"}
 - Change "Show Details" link to a delete link.
 - Display "Alert sent" if `message_sent` is `true` before the copy, otherwise display "Alert not yet sent".
 - Instead of showing all of the user's flights, show only upcoming flights. Review your advanced `.where` techniques:

    [https://chapters.firstdraft.com/chapters/770#less-than-or-greater-than](https://chapters.firstdraft.com/chapters/770#less-than-or-greater-than){:target="_blank"}
 - Add a past flights section.

### Custom rake tasks

After getting the interface for users to add and delete flights into reasonable shape, create a file in `lib/tasks/` called `reminders.rake`. Within it, add this code:
 
```ruby
task({ :send_reminders => :environment }) do
  p "Howdy"
  p "World!"
end
```

Then, at a command prompt, run the command `rails send_reminders`. You should see `Howdy!"

This is known as a **custom rake task**. These are nothing more than arbitrary Ruby programs that you can write and run whenever you want, but they are better than plain old `.rb` files because they are aware of your entire Rails application — most importantly, you have access to your models and gems. (Whenever you've been running `rails dummy_data` in the past, you've been running a rake task that I've written for you that pre-populates your tables.)

Write some Ruby within the task that:

 - Finds flights where messages have not yet been sent. Print how many there are.
 - Finds flights that depart within the next 24 hours and 15 minutes. Print how many there are. Review your advanced `.where` techniques:

    [https://chapters.firstdraft.com/chapters/770#less-than-or-greater-than](https://chapters.firstdraft.com/chapters/770#less-than-or-greater-than){:target="_blank"}
 - Finds flights where messages have not yet been sent AND depart within the next 24 hours and 15 minutes. Print how many there are. Review how to chain `.where`s together:

    [https://chapters.firstdraft.com/chapters/770#chaining-wheres](https://chapters.firstdraft.com/chapters/770#chaining-wheres)

Now that we have the correct set of flights, the ones that we need to send reminders for, 

 - Loop through them and print each one's description and departure time.
 - In addition, print each one's user's phone number.
 - Finally, in addition, also update their `sent_message` to `true`.
 - Visit the flights index page in the web interface and confirm that it worked. Add another couple of flights, run the task again, and confirm that it's working as intended.

### Dealing with timezones

You might have noticed that the times look off; this is because times are, by default, saved in Coordinated Universal Time (UTC), or Greenwich Mean Time. I.e., 6 hours ahead of Chicago time.
    
Properly dealing with timezones is tricky, although Rails does make it as painless as possible. For now, I recommend just assuming all of your users are in Chicago, if that's feasible for your app idea. If so, you can do the following. In `config/application.rb`, add the following lines somewhere:

```ruby
config.time_zone = 'Central Time (US & Canada)'
config.active_record.default_timezone = :local
```

This is not a great idea in real-world apps, because you should always store times in UTC in the database, and then transform them in the views to whatever timezone and format the user wants to see. But for our proof-of-concept, this hack will do.

### Styling with Bootstrap

You can make things more attractive using Bootstrap and Font Awesome:

 - https://getbootstrap.com/docs/4.4/components/card/#list -groups
 - https://getbootstrap.com/docs/4.4/components/badge/
 - https://getbootstrap.com/docs/4.4/utilities/flex/
 - https://fontawesome.com/icons?d=gallery&m=free


In order to get the Bootstrap and Font Awesome external stylesheets `<link>`ed to your HTML documents, there's a handy generator. From a command prompt,

```
rails generate draft:layout
```

This will replace your default `app/views/layouts/application.html.erb` file with one that contains `<link>`s to Bootstrap and Font Awesome. Before it does so, it will warn you that it is about to overwrite your existing one; if you wrote anything in there that you want to keep, go copy it and put it somewhere safe. Then say "yes" and allow the generator to overwrite.
