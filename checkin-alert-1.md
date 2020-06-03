# CheckIn Alert — Problem Definition

## Pain point

I fly on Southwest Airlines frequently. Southwest Airlines does not have assigned seats; instead, passengers are assigned a boarding number, line up in that order, and then can choose whatever seat is available once they are on the plane.

Usually people with a low number (those in groups "A" and "B") grab the aisle and window seats, and those at the end (C group) are stuck in the middle seats and also have to fight over the remaining overhead bin space. So A and B groups are highly sought after.

You can, of course, pay extra to get a low number. But after that, boarding order is determined by who checks in first. The online check-in period begins 24 hours before departure time, so it's optimal to check in _exactly_ 24 hours before your departure time to get the best possible boarding number.

Unfortunately, I always forget to do this and always end up in the C group. The Southwest iPhone app sends a push notification to check in online, but that comes about 10 minutes too late, after everyone else has already checked in.

I wish that I could be reminded by a text message 5-10 minutes _before_ it's time to check in, so that I can be ready to beat everyone else!

## Sketches / domain model

Our goal is to think of the simplest possible _proof of concept_ application that we can build to test out a solution to this problem. Ultimately, our goal is to have a complete domain model (all tables, all columns) so that we can generate our resources and get started coding.

 - First, I like to sketch out the screens that I envision in my app.
 - I  _ruthlessly_ eliminate features that are not essential.
 - Then, I think about the tables and columns I need to back these screens (and every link/form that I can click in these screens).
 - After a few rounds of going back and forth between my sketches, my feature list, and my database design, I hope to end up with a complete Entity Relationship Diagram / domain model.

The one slightly non-CRUD technique this app will involve is running a rake task automatically every 10 minutes[^scheduler]. We'll use this technique to, every 10 minutes, send out reminders to anyone who is due to receive one and hasn't already been reminded.

[^scheduler]: It hardly needs teaching; once we have the rake task written and [deployed the app to Heroku](https://chapters.firstdraft.com/chapters/775), we use [the Scheduler add-on](https://devcenter.heroku.com/articles/scheduler){:target="_blank"}. This involves running two commands:

    ```
    heroku addons:create scheduler:standard
    heroku addons:open scheduler
    ```

    And then you select the rake task and the frequency with which you want Heroku to run it:

    ![](/assets/heroku-scheduler.png)

    That's it! Heroku will take care of the rest.

Our first task will be to build a CRUD app to collect the information we need to run this program every 10 minutes. Take a few minutes and see if you can sketch out a quick design for a solution to this problem.

Then, try to domain model it and draw out an ERD at [ideas.firstdraft.com](https://ideas.firstdraft.com/).
