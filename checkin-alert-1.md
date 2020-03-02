# CheckIn Alert — Problem Definition

## Pain point

I fly on Southwest Airlines frequently. Southwest Airlines does not have assigned seats; instead, passengers are assigned a boarding priority and then can choose whatever seat is available.

Usually people with a high boarding priority (A and B groups) grab the aisle and window seats, and those at the end (C group) are stuck in the middle seats and also have to fight over the remaining overhead bin space. So A and B groups are highly sought after.

You can, of course, pay extra to get a high boarding priority. But after that, boarding order is determined by who checks in first. You are allowed to check in online 24 hours before your flight departs, so it's optimal to check in _exactly_ 24 hours before your departure time to get the best possible boarding priority.

Unfortunately, I always forget to do this and always end up in the C group. The Southwest iPhone app sends a push notification to check in online, but that comes about 10 minutes too late, after everyone else has already checked in.

I wish that I could be reminded by a text message 5-10 minutes _before_ it's time to check in, so that I can beat everyone else!

## Sketches / domain model

Our goal is to think of the simplest possible _proof of concept_ application that we can build to test out a solution to this problem. Ultimately, our goal is to have a complete domain model (all tables, all columns) so that we can generate our resources and get started coding.

 - First, I like to sketch out the screens that I envision in my app.
 - I eliminate features that are not essential ruthlessly.
 - Then, I think about the tables and columns I need to back these screens (and every link/form that I can click in these screens).
 - After a few rounds of doing this, I hope to end up with a complete Entity Relationship Diagram / domain model.

The one new technique I'm going to show you today is: how to write a standalone Ruby program and have it run automatically every 10 minutes. (As opposed to waiting for a user to visit a URL and then triggering an action.) We'll use this technique to send out reminder text messages (using Twilio) every 10 minutes to anyone who is due to receive one.

Given that, take a few minutes and see if you can come up with a design and a domain model for a solution to this problem. Draw out an ERD at [ideas.firstdraft.com](https://ideas.firstdraft.com/).