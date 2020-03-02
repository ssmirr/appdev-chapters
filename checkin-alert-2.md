# CheckIn Alert — Project Notes

## Target

[https://checkin-alert.matchthetarget.com/](https://checkin-alert.matchthetarget.com/)

## Tasks

 - First, explore the target and ask questions about its features.
 - Then, domain model — figure out the entire database structure: all tables and all columns. What associations are there? What foreign key columns and/or join tables? For each column, what validation rules, if any?
 - Make an entity relationship diagram in [firstdraft Ideas](https://ideas.firstdraft.com/).
 - Then, create a brand new Rails application. Click "Show Tutorial" at the bottom of your ERD for some help.
 - Create the standard CRUD infrastructure for flights, and the standard sign-up/sign-in/sign-out infrastructure for users.
 - If you want to, you can make things more attractive using Bootstrap and Font Awesome:
    - https://getbootstrap.com/docs/4.4/components/card/#list-groups
    - https://getbootstrap.com/docs/4.4/components/badge/
    - https://getbootstrap.com/docs/4.4/utilities/flex/
    - https://fontawesome.com/icons?d=gallery&m=free
    
    In order to get the Bootstrap and Font Awesome external stylesheets `<link>`ed to your HTML documents, there's a handy generator. From a command prompt,
    
    ```
    rails generate draft:layout
    ```
    
    This will replace your default `app/views/layouts/application.html.erb` file with one that contains `<link>`s to Bootstrap and Font Awesome. Before it does so, it will warn you that it is about to overwrite your existing one; if you wrote anything in there that you want to keep, go copy it and put it somewhere safe. Then say "yes" and allow the generator to overwrite.
 - Nav:
    - Sign in/Sign up when no one is signed in
    - Edit profile/Sign out when someone is signed in
 - Add a new flight form:
    - Get rid of user ID input; it should use the signed in user
 - List of flights:
    - The signed in user should see only their own flights, not everyone's flights.
    - Make it an unordered list instead of a table.
    - Format the copy.
        - For formatting the time, there's a method called `.strftime()`.
        - The argument to `.strftime` is a string that contains a formatting pattern. For example, `f.departs_at.strftime("%I:%M%p")`.
        - 

        Departs for West Nickolestad at 8:22am on Nov 22, 2019
    - Format the time using the `.strftime` method. The method is called on any `Time`, `Date`, or `DateTime` object and requires a `String` argument that determines how to format the time. For example,
    
        ```ruby
        photo.created_at.strftime("%a %b %Y")
        ```
        
        would produce something like "Fri Nov 2019". The time formatting codes are a pain to remember, so there are many handy tools to help compose them. I like this one: http://strftime.net/
    - Change "Show Details" link to a delete link.
    - Display "MESSAGE SENT" if `message_sent` is `true` before the copy, otherwise display "MESSAGE NOT SENT".
    - Break up the list into two lists: Upcoming flights and Past flights.
 - After getting the interface for users to add and delete flights into reasonable shape, create a file in `lib/tasks/` called `send_reminders.rake`. Within it, add this code:
 
    ```ruby
    task({ :send_sms => :environment }) do
      p "Howdy!"
    end
    ```
    
    Then, at a command prompt, run the command `rails send_sms`. You should see `Howdy!"
    
    This is known as a **custom rake task**. These are nothing more than arbitrary Ruby programs that you can write and run whenever you want, but they are better than plain old `.rb` files because they are aware of your entire Rails application — most importantly, you have access to your models and gems. (Whenever you've been running `rails dev:prime` in the past, you've been running a rake task that I've written for you that pre-populates your tables.)
    
    Write some Ruby within the task that:
    
    - Finds flights where messages have not yet been sent. Print how many there are.
    - Finds flights that depart within the next 24 hours and 10 minutes. Print how many there are.
    - Finds flights where messages have not yet been sent AND depart within the next 24 hours and 10 minutes. Print how many there are.
    - Loop through all of the previous set of flights and print their description and departure time.
    - Finally, the last step would be to send the text message and then update `sent_message` to `true`.
        
        You can read the docs for the twilio-ruby gem [here](https://github.com/twilio/twilio-ruby), but an abridged version follows:
        
        Find the `Gemfile` in the top-level folder of the app and add the line:
            
        ```ruby
        # Gemfile

        gem "twilio-ruby"
        ```

        Then, at a command prompt,

        ```
        bundle install
        ```

        Now, anywhere in your application, you have access to the `Twilio::REST::Client` class, which will do all of the talking to Twilio's API for you. To send an SMS, for example, you can use the `.api.account.messages.create` method as follows (assuming you've [stored the appropriate credentials in environment variables](https://www.gitpod.io/docs/47_environment_variables/#using-the-dashboard)):

        ```ruby
        twilio_sid = ENV.fetch("TWILIO_ACCOUNT_SID")
        twilio_token = ENV.fetch("TWILIO_AUTH_TOKEN")

        sms_parameters = {
          :from => ENV.fetch("TWILIO_ASSIGNED_PHONE_NUMBER"),
          :to => +19876543210,
          :body => "Craft your message here"
        }

        # Set up a client to talk to the Twilio REST API
        twilio_client = Twilio::REST::Client.new(twilio_sid, twilio_token)

        # Send your message through the client
        twilio_client.api.account.messages.create(sms_parameters)
        ```
    - You might have noticed that the times look off; this is because times are, by default, saved in Coordinated Universal Time (UTC), or Greenwich Mean Time. I.e., 6 hours ahead of Chicago time.
    
        Properly dealing with timezones is tricky, although Rails does make it as painless as possible. For now, I recommend just assuming all of your users are in Chicago, if that's feasible for your app idea. If so, you can do the following. In `config/application.rb`, add the following lines somewhere:
    
        ```ruby
        config.time_zone = 'Central Time (US & Canada)'
        config.active_record.default_timezone = :local
        ```
    
        This is not a great idea in real-world apps, because you should always store times in UTC in the database, and then transform them in the views to whatever timezone and format the user wants to see. But for our proof-of-concept, this hack will do.