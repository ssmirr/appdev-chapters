# Data integrity with validations

As soon as we start saving data into our tables from external sources, be it from our users, CSVs, APIs, or wherever, we have to start worrying about whether that data is _valid_. Did they fill out all of the required fields? Did they choose a username that was already taken? Did they enter an age less than 0? Did they vote twice?

If _invalid_ records sneak into our database, and our code assumes our data is valid, then we're going to have all kinds of problems. Then, we have to start writing our code very defensively, with tons of `if`/`elsif`/`else`/`end` statements scattered everywhere to guard against invalid _data_ causing errors in otherwise functional code.

For example, in this project (after `rails sample_data` and `bin/server`), visit the details page of a movie; say, `/movies/2`. If you just ran `rails sample_data`, you should see the details page for The Godfather.

Now, in another tab, visit `/rails/db` and delete the director of this movie, Francis Ford Coppola.

Now, visit our the details page we've worked so hard on, `/movies/2` again. `undefined method 'name' for nil:NilClass`?! What's up with that? It was fine a second ago!

Should you go change Line 51 of `app/views/movie_templates/show.html.erb` to resolve the issue? All too often, this is what students do. But you don't have a code problem; you have a _data_ problem.

Even worse, go back to `/movies`. The entire index page is broken, due to a handful of orphaned movies within the `.each` loop!

Fortunately, we have several tools at our disposal to solve the immediate problem at hand:

 - Use the debugging REPL embedded in the Better Errors page to delete the movies that are causing problems, or assign them valid `director_id`s and save.
 - Probably better: run `rails sample_data` again to reset everything.

## Defensive code

Sometimes, we really _do_ want to allow spotty data, and we just have to be prepared for it when writing our code. In this case, maybe we want to allow movies to be created without the creator knowing who the director is, and so they are allowed to leave `director_id` blank.

In that case, you have to write your code defensively, with `if`/`else`/`end` statements. For example, we could do something like this:

```erb
<% if @the_movie.director != nil %>
  <%= @the_movie.director.name %>
<% else %>
  Uh oh! We weren't able to find a director for this movie.
<% end %>
```

This will get very tiresome to repeat over and over, so this too might be best encapsulated as an instance method:

```ruby
class Movie < ApplicationRecord
  def director
    my_director_id = self.director_id

    matching_directors = Director.where({ :id => my_director_id })
    
    the_director = matching_directors.at(0)

    return the_director
  end

  def director_name_or_uh_oh
    if self.director != nil
      return self.director.name
    else
      return "Uh oh! We weren't able to find a director for this movie."
    end
  end
end
```

Now, we can use this method wherever we need a director name or apology:

```erb
<%= @the_movie.director_name_or_uh_oh %>
```

## Data integrity with validations

Many times, we don't want to allow spotty data; we want our data to be uniform and consistent. In this case, it's best to never allow invalid data that doesn't meet our criteria to ever enter our database in the first place; then, we don't need to worry about writing lots of defensive conditionals downstream in our code.

If so, then ActiveRecord offers a tremendously useful feature: **validations**.



Validations are a way to declare in the model what rules we have for each column as far as what data is allowed in it.

