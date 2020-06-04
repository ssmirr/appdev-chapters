# Ransack Cheatsheet

The Ransack gem provides us with a powerful, flexible, easy-to-integrate search/filter form.

## Installation

In your Gemfile, include

```ruby
gem 'ransack'
```
    
and then `bundle install`. Restart your `rails server`.

## Simple, single-table filtering

First, modify your controller action from:

```ruby
def index
  @movies = Movie.all
end
```

to:

```ruby
def index  
  @q = Movie.ransack(params[:q])
  @movies = @q.result
end
```

Next, add a search form to the view template:

```erb
<%= search_form_for @q do |f| %>
  <p>Narrow results:</p>

  <div>
    <%= f.label :title_cont, "Title containing" %>
    <%= f.text_field :title_cont, :placeholder => "Enter a few characters" %>
  </div>

  <div>
    <%= f.label :year_gteq, "Released after" %>
    <%= f.text_field :year_gteq, :placeholder => "Year greater than or equal to" %>
  </div>

  <div>
    <%= f.label :year_lteq, "Released before" %>
    <%= f.text_field :year_lteq, :placeholder => "Year less than or equal to" %>
  </div>

  <%= f.submit :class => "btn btn-primary btn-block" %>
  
  <a href="/movies">Clear filters</a>
<% end %>

```

From the above example, you need to customize the following:

 1. **Crucially**, the names of the inputs (in the example, `:title_cont`, `:year_gteq`, and `:year_lteq`). The names are very particular: the first part needs to be the name of the column you want to search, and then an underscore, and then a *predicate* that indicates what kind of search you want. The most common ones are `_eq` (exactly equals), `_cont` (contains), `_gteq` (greater than or equal to), and `_lteq` (less than or equal to). [Here is a full list of all available predicates.](https://github.com/activerecord-hackery/ransack/wiki/Basic-Searching#eq-equals) Add fields for however many columns you want to allow users to narrow by.
 2. The labels (in the example, `"Title containing"`, `"Released after"`, and `"Released before"`).
 3. The placeholder text.

## Filtering by associated tables

If you want to allow your users to narrow by attributes of associated objects, change your action to look like this:

```ruby
def index  
  @q = Movie.ransack(params[:q])
  @movies = @q.result(:distinct => true).includes(:director, :actors)
end
```

In your app, customize `.includes(:director, :actors)` with the relevant associations that you want to include in the query.

In the view, you can now add the additional inputs to the search form:

```erb
<div>
  <%= f.label :director_name_cont %>
  <%= f.text_field :director_name_cont %>
</div>

<div>
  <%= f.label :actors_name_or_actors_bio_cont %>
  <%= f.text_field :actors_name_or_actors_bio_cont %>
</div>

<div>
  <%= f.label :actors_id_eq, "Actor" %>
  <%= f.select :actors_id_eq, options_from_collection_for_select(Actor.all, :name, :name, @q.actors_id_eq), { :include_blank => true } %>
</div>
```

A few things to note:

 1. The new input names start with the association, then the column (`:director_name_...`, `:actors_name_...`). **Pluralization matters.**
 2. You can chain multiple columns together with the same criterion by using `or` (`:actors_name_or_actors_bio_cont`).
 3. If you prefer a dropdown over a text field, you can do it as shown above for actors.
 4. If you prefer checkboxes, you can use code like this:

```erb
<div>
  <% Actor.all.each do |actor| %>
    <label>
      <%= check_box_tag('q[actors_id_eq_any][]', actor.id, (true if @q.actors_id_eq_any.try(:include?, actor.id))) %>
      <%= actor.name %>
    </label>
  <% end %>
</div>
```

### Further Reading

 - [Official docs](https://github.com/activerecord-hackery/ransack)
 - [Railscast #370](http://railscasts.com/episodes/370-ransack)

Happy filtering!

> For actual intelligent, fuzzy, _searching_, things get more involved; options include Postgres's built-in search (via a gem like [pg_search](https://github.com/Casecommons/pg_search)), Elasticsearch (via a gem like [SearchFlip](https://github.com/mrkamel/search_flip)), or [Algolia](https://www.algolia.com/doc/framework-integration/rails/getting-started/setup/?language=ruby). All of these are relatively advanced techniques that require some re-configuration of the database itself.
