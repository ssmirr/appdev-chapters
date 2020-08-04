# Refactoring MSM Queries with Methods

This chapter is the companion to [the refactoring-msm-queries-1 project](https://github.com/appdev-projects/refactoring-msm-queries-1){:target="_blank"}, which is the sequel to the [the msm-queries project](https://github.com/appdev-projects/msm-queries){:target="_blank"}.

## Objective

Our goal is to keep msm-queries working exactly the same way that it was after we finished building it; we're not going to add a thing. Therefore, we'll use the exact same target as before:

[https://msm-queries.matchthetarget.com/](https://msm-queries.matchthetarget.com/){:target="_blank"}

Our starting point code for this project, refactoring-msm-1, is one solution for msm-queries. But we're going to make the code much more modular and re-usable, while keeping the functionality exactly the same. How? By **defining methods** to encapsulate our querying logic.

So, this might be a good time for you to do two things:

 - Read through the starting point code and compare it to your own solution to msm-queries. `rails sample_data` and `bin/server` so that you can click through the application, verify that it's working, and read the server log.
  
    Are there any differences between the starter code and your own solution to msm-queries? You will most likely find at least one or two. What are they doing? Practice _reading_ the code and reasoning your way through it, line by line; explain it to yourself, or to your rubber ducky. Developers read far more code than we write.
    
    Does any part of the code puzzle you?
 - A quick review of the section on how we [define methods](https://chapters.firstdraft.com/chapters/769#defining-instance-methods){:target="_blank"}.

Go ahead, I'll wait!

## The problem: queries all over the place

Right now, there are several places in our application where we have a movie and we want to display something about the director of the movie.

For example, in `app/views/movie_templates/show.html.erb`, we have an instance of `Movie` in a variable called `@the_movie`, and we want to display the name of the director. So, first, we use the value in the attribute `@the_movie.director_id` to look up a matching record in the directors table:

```erb
<% matching_directors = Director.where({ :id => @the_movie.director_id }) %>
    
<% the_director = matching_directors.at(0) %>
```

Then, finally, we can get the value in the `name` or `dob` or `bio` or whatever other attribute we care about from that instance of `Director`:

```erb
<%= the_director.name %>
```

This same basic logic — given a movie, use its `director_id` attribute to look up a row in the directors table — is repeated in several places: the movies index page, an actor's filmography, etc. If we were to continue building this application out more, no doubt we would be doing this task many more times; nearly everywhere a movie appears, or in the `rails console`, or in a rake task, along with information about the movie itself (like `title`, `year`, etc), we will want to display information about the associated director (like `name`, etc).

What a pain to have to repeat this query, `Director.where({ :id => @the_movie.director_id }).at(0)`, every single time I just want some info about a movie's director!

## The solution: encapsulate queries in methods

Let's make life easier on our future selves and on all our future teammates: **let's define a nicely named instance method** that will do this work and that we can call on any instance of `Movie` whenever we want to.

The method will encapsulate what we've been doing repetitively over and over until now: it will talk to the `Director` class, retrieve the record from the directors table corresponding to the movie's `director_id`, and return an instance of `Director` to us. Then, we can use `Director` attribute accessors to get whichever column values we are interested in about the director.

Since we named our foreign key column in the movies table `director_id`, we automatically got an attribute accessor method on `Movie` called `.director_id` from ActiveRecord which returns an `Integer`. What shall we call our new method that we will define, which will _use_ the `Integer` returned by `.director_id` to retrieve an instance of `Director` and return that instead?

How about `.director`?

Here's what I wish we could do. If we have an instance of `Movie` inside a variable called `@the_movie`, instead of[^protecting_against_nil]:

[^protecting_against_nil]: In the actual starter code, you'll notice that the code looks slightly different:

    ```erb
    <% matching_directors = Director.where({ :id => @the_movie.director_id }) %>

    <% the_director = matching_directors.at(0) %>

    <% if the_director != nil %>
      <%= the_director.name %>
    <% else %>
      Uh oh! We weren't able to find a director for this movie.
    <% end %>
    ```

    Why do you think this is? Which error message does this protect against?

    Well: If someone deletes a movie's director, what would happen to the movie's details page if the `if`/`else`/`end` statement _wasn't_ there? Evaluate the code, line by line, as if _you_ were Ruby and had to follow the code's instructions.

    Without the guard of the `if the_director != nil`, once you reached `the_director.name`, you should have said "next, call the `.name` method on `nil` — oops, there's no `.name` method for `NilClass`! Did you mean `Director`?"

    That's because `Director.where({ :id => @the_movie.director_id })` would have returned an empty `ActiveRecord::Relation`, and when you try to access any element of an empty array with `.at()`, it returns `nil`.

```erb
<% matching_directors = Director.where({ :id => @the_movie.director_id }) %>
    
<% the_director = matching_directors.at(0) %>

<%= the_director.name %>
```

I want to be able to do:

```erb
<% the_director = @the_movie.director %>

<%= the_director.name %>
```

Or, if we're okay with chaining a couple of methods on the same line, we could even do:

```erb
<%= @the_movie.director.name %>
```

Ahhhhh! Wouldn't that be nice? Unfortunately, if you try it right now, you'll get a big red "Undefined method `director` for `Movie`" error. If we want a handy method like that, we'll have to roll up our sleeves and define it.








You shouldn't be worrying about writing that database query over and over and over and over again while you are crafting your user interface. Or, more realistically, on a multi-person team, the
