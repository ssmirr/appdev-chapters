# Photogram Final

Here is your target:

[https://photogram-final.matchthetarget.com/](https://photogram-final.matchthetarget.com/){:target="_blank"}

## Hints

The starting point of this project is a blank Rails application. You need to create all the tables and models yourself!

We domain modeled this application in first two weeks of class, and used the same database structure in photogram-gui and photogram-signin; here's the Entity Relationship Diagram (ERD):

![](/assets/photogram-final-erd.png)

If you add these columns _exactly_, then you can run the `rails sample_data` Rake task to add dummy data to your database tables like with the homeworks.

You can use the `model` generator to add your tables, or you can use [the `draft_generators`](https://chapters.firstdraft.com/chapters/773) to add the tables _and_ give yourself a big head start on routes, controllers, actions, and view templates.

You should also consider adding [association accessor methods](https://association-accessors.firstdraft.com/) to your models to make your job easier. [Validations to protect data integrity](https://chapters.firstdraft.com/chapters/845) are important as well.

### Follow Requests

When a `FollowRequest` is created you need to check if the recipient of the request is a private User. If they are private, the `status` column of the `FollowRequest` should be set to `"pending"`. If they are _not_ private the `status` should be set to `"accepted"`.

If a user has a private account, all `"pending"` received `FollowRequests` should appear on their details page with the option to accept. Accepting should change the `status` from `"pending"` to `"accepted"`, while rejecting should set the `status` to `"rejected"`.

### Feed 

Clicking the **Feed** link should show all of the photos from every person that the user is following. For example, if I am following Jelani, I should see all of Jelani's posted photos. 

### Discovery

Clicking the **Discovery** link should show all of the photos liked by the people the user is following. For example, if I am following Jelani and he has liked seven different photos, all of those photos should be visible in my discovery section. 

### Image Uploads

In the target, users add images by uploading files instead of entering image URLs into a text field. Please see the chapter on [images uploads](https://chapters.firstdraft.com/chapters/790) for more information.
