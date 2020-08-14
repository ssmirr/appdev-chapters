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
