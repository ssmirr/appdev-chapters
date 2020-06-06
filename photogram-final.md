# Photogram Final

Here is your target:

[https://photogram-final.matchthetarget.com/](https://photogram-final.matchthetarget.com/){:target="_blank"}

## Hints

The starting point of this project is a blank Rails application. You need to create all the tables and models yourself!

We domain modeled this application in first two weeks of class, and used the same database structure in photogram-gui and photogram-signin; here's the Entity Relationship Diagram (ERD):

![](/assets/photogram-final-erd.png)

If you add these columns _exactly_, then you can run the `rails dummy_data` Rake task to add dummy data to your database tables like with the homeworks.

You can use the `model` generator to add your tables, or you can use [the `draft_generators`](https://chapters.firstdraft.com/chapters/773) to add the tables _and_ give yourself a big head start on routes, controllers, actions, and view templates.

Remember that the [photogram-gui](https://github.com/appdev-projects/photogram-gui/blob/master/app/models/photo.rb){:target="_blank"} and [photogram-signin](https://github.com/appdev-projects/photogram-signin/blob/master/app/models/user.rb){:target="_blank"} projects had _very_ helpful instance methods already defined in the model files; go study those methods if you haven't already. You might want to define methods like those before you begin building out your interface; it will make traversing your relationships a snap.
