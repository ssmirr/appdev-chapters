
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
