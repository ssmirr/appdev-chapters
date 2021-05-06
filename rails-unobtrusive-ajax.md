# Ajax with Rails UJS

## Objective

Now that we've learned [a minimal amount of JavaScript](https://chapters.firstdraft.com/chapters/861){:target="_blank"}, let's apply our skills to our Rails applications to make them _slick_.

These notes are a companion to the [pg-ajax-1 project](https://github.com/appdev-projects/pg-ajax-1){:target="_blank"}.

[This is our target.](https://pg-ajax-1.matchthetarget.com/){:target="_blank"}

## Comments

Let's focus first on the experience of CRUDing comments. Right now if you add, update, or destroy a comment, the `comments#create`, `comments#update`, and `comments#destroy` actions use the `redirect_back` method to send you back to the page you were previously on; which is pretty cool, actually. Much better than `redirect_to root_url` no matter where you were before.

But, you end up all the way at the top of the page; of course, since redirecting is the same as if you navigated to the page by typing in the address manually. This is very annoying if you had scrolled a long way down the feed, just wanted to leave a comment or delete a comment, and continue browsing.

Let's improve this experience.

### Ajaxify destroy

We'll begin by improving the delete flow.

#### Change the link from submitting an HTML request to a JS request

The first thing we need to do is make it so that when the user clicks on the delete link, it _doesn't_ actually do what `<a>` elements are supposed to do, which is navigate them to the URL specified by the `href=""` attribute. We want to keep them right where they are; in essence, we need to break the link.

Secondly, we need to attach a custom JavaScript event handler to the link such that when it is clicked, we will make the request to the route that the user _would_ have navigated to using JavaScript. This will trigger the action so that the appropriate CRUD will still occur.

Finally, the request that is placed must be of the format `.js`, rather than `.html`. Then, we can `respond_to` it accordingly.

Ok, phew. How are we going to do all this? Well, fortunately the `link_to` and `form_with` methods are going to do it all for us; but just as a thought experiment, here's what we'd need in order to do it ourselves:

 - The [jQuery ajax() method](https://api.jquery.com/jquery.ajax/){:target="_blank"}. It is the equivalent of when we used [`HTTP.get()` or `HTTP.post()`](https://github.com/httprb/http#basic-usage){:target="_blank"} to interact with APIs in Ruby; you provide it with a URL and it places a request to that URL.
 - We can `return false` from `on("click")`'s callback, or use [the preventDefault method](https://api.jquery.com/event.preventdefault/){:target="_blank"}, to prevent the user from going anywhere when they click the link.

With these pieces, we _could_ write the code ourselves; it would look something like the following:

```erb
<%= link_to "#", # comment,                      # removed the href since we're breaking the link anyway
      # method: :delete,                         # removed the method since we're breaking the link anyway
      class: "btn btn-link btn-sm text-muted",
      id: "#comment_{comment.id}_delete_link" do # added an id to the link so that I can bind a click handler to it %>
  
  <i class="fas fa-trash fa-fw"></i>
<% end %>

<script>
  // bind the click handler
  $("#comment_<%= comment.id %>_delete_link").on("click", function() { 
    
    $.ajax({
      url: "/comments/<%= comment.id %>", // what URL to submit a request to
      type: "DELETE",                     // make it a DELETE request
      dataType: "script"                  // what is the format of the request
    });

    return false;                         // break the link
  });
</script>
```

Fortunately, we don't have to write the above code for every link we want to _Ajaxify_. Instead, we can add a _very_ handy option on the `link_to` helper method, which does the equivalent for us — `remote: true`:

```erb
<%= link_to comment,
      method: :delete,
      class: "btn btn-link btn-sm text-muted",
      remote: true do %>

  <i class="fas fa-trash fa-fw"></i>
<% end %>
```

Add the `remote: true` option, clear your server log, click the link, and observe the server log. You'll notice that 1) the link didn't seem to do anything when clicked, but 2) the request did in fact hit the correct route, 3) the request format was `as JS` rather than `as HTML` like usual, and 4) if you refresh the page, the comment was in fact deleted (since the action was triggered).

Great! Now all we have to do is update the HTML with jQuery to keep the client in sync with the database.

#### respond_to JS

In the `comments#destroy` action, let's expand the `respond_to` block to handle requests for format `JS`:

```ruby
respond_to do |format|
  # Handle JSON and HTML formats above as usual

  format.js do
    render template: "comments/destroy.js.erb"
  end
end
```

Now, as usual, let's go create this template file in `app/views/comments`:

```js
// app/views/comments/destroy.js.erb

console.log("bye comment!")
```

Now try clicking delete on a comment. You ought to see `bye comment!` in the JS console — your template is being rendered, but it's a _JavaScript_ template that is being _executed_ by the browser.

#### View Source for JS responses

**A very important note:** Since your templates are now JavaScript, one of your primary debugging tools — looking at HTML with **View Source** — can no longer help you. How do you see the actual JavaScript that your templates are producing and sending to the browser?

Chrome has your back. Go to the Network tab in the Dev Tools:

![](/assets/js-view-source.png)

This will be _crucial_ to use as we move along. You will definitely, 💯, absolutely make typos in the jQuery selectors that you try to compose using ERB tags in these templates, and it's essential that you use the Network tab to debug.

#### Remove the comment

Now, let's use our jQuery skills to actually remove the comment from the DOM.

##### Add a unique ID to the component

First, let's make it easy on ourselves by putting a unique `id=""` on it. Then it will be easy to select with `$()`:

```erb
<!-- app/views/comments/_comment.html.erb -->

<li id="comment_<%= comment.id %>" class="list-group-item">
```

Which would produce something like:

```html
<li id="comment_42" class="list-group-item">
```

Which would be easily selectable with `$("#comment_42")` so that we can `hide()` it or whatever.

`comment_<%= comment.id %>` was not hard at all, but since we're going to end up doing this a lot, Rails includes a helper method for it — `dom_id`:

```erb
<li id="<%= dom_id(comment) %>" class="list-group-item">
```

Which does the exact same thing — essentially, `dom_id(thing)` returns:

```ruby
"#{thing.class.to_s.underscore}_#{thing.id}"
```

##### Remove the component from the DOM

Now that it's easy to select, let's grab it with jQuery and remove it. If our goal is to, ultimately, execute some jQuery that looks like this:

```js
$("#comment_42").remove();
```

But where is the `42` going to come from? Remember, since this is a `.js.erb` template, we can 1) embed Ruby into it anywhere we please, and 2) we have access to any instance variables defined by the action; just like with `.html.erb` templates!

```js
// app/views/comments/destroy.js.erb

$("#<%= dom_id(@comment) %>").remove();
```

Give your Ajaxified delete link a whirl. Nice!

But it seems a little abrupt. Now that we have all of jQuery at our disposal, why not be a little smoother?

```js
// app/views/comments/destroy.js.erb

$("#<%= dom_id(@comment) %>").fadeOut(5000, function() {
  $(this).remove();
});
```

Okay, five seconds might be excessive, but you get the idea.

Congrats on Ajaxifying your first interaction!

### Ajaxify create

Let's improve the experience of adding a comment. It will follow the same pattern as deleting:

 1. Switch the request (in this case, a form instead of a link) from HTML to JS.
 2. Update the `respond_to` block to handle requests for JS.
 3. Write a JS response template.

#### Switch the request from HTML to JS

Add the `local: false` option to the `form_with` helper that is rendering the form to add a comment:

```erb
<!-- app/views/comments/_form.html.erb -->

<%= form_with(model: comment, local: false) do |form| %>
```

Submit the form and verify that the form no longer navigates, the request is still made in the background, and the format of the request is now `JS` instead of `HTML`.

#### Update the respond_to block

In the `comments#create` action, let's expand the `respond_to` block to handle requests for format `JS`:

```ruby
respond_to do |format|
  # Handle JSON and HTML formats above as usual

  format.js do
    render template: "comments/create.js.erb"
  end
end
```

If you want to, you can be more concise here — since:

 - The view folder name matches the controller name.
 - The template name matches the action name.
 - The request format matches the file extension.

We can just say:

```ruby
respond_to do |format|
  # Handle JSON and HTML formats above as usual

  format.js
end
```

With an empty block, or no block at all, and Rails will be able to find our template file.

#### Write the template file

Create the template file in `app/views/comments`:

```js
// app/views/comments/create.js.erb

console.log("howdy")
```

And make sure you wired everything up correctly. Once you've verified that `howdy` appears in the console when you add a new comment, try printing the content of the new comment:

```js
console.log("<%= @comment.body %>")
```

Once you've proven that you're sending the data back, think about what interaction you'd like to use to actually update the client. [Review my list of frequently used jQuery methods](https://chapters.firstdraft.com/chapters/861#frequently-used-jquery-methods){:target="_blank"} and see if any might come in handy.

At this point, it's really up to you to use your JavaScript skills to craft a JavaScript response that fits your application's context. But, let me show you a pattern that has served me well in many cases.

---

First, recall that we can pass a string containing HTML directly to the `$()` method to create an element. How about if we do that with the comment's body, perhaps within a `<p>` for now, and then use the `before()` jQuery method to insert the comment into the DOM just before the form for a new comment?

Let's try it. First, we'll need to add a way to select the `<form>`, so that we can call `before()` on it. Let's try adding a `dom_id` to it:

```erb
<!--  app/views/comments/_form.html.erb -->

<li id="<%= dom_id(comment) %>_form" class="list-group-item">
```

In the `_form` partial for a new comment, `comment` is a brand new, unsaved comment. So `dom_id(comment)` doesn't have an ID number to work with. So it returns `new_comment`. Therefore, the above would produce:

```html
<li id="new_comment_form" class="list-group-item">
```

You can try it, and try something like this in your response:

```js
var added_comment = $("<p><%= @comment.body %></p>");

$("#new_comment_form").before(added_comment);
```

But it wouldn't work quite right, because there can be _multiple_ new comment forms on the page, so the new comments would be appended to the wrong one. We need a more specific selector.

One very good option would be to add a more specific selector to the element:

```erb
<li id="<%= dom_id(comment.photo) %>_new_comment_form" class="list-group-item">
```

And update the JS template accordingly:

```js
var added_comment = $("<p><%= @comment.body %></p>");

$("#<%= dom_id(@comment.photo) %>_new_comment_form").before(added_comment);
```

Another very good option would be to [learn to write more specific CSS selectors using the existing structure of the page](https://flukeout.github.io/){:target="_blank"}; it might come in handy if you go deep into Ajax, as it did when web scraping.

Either way — we now have the comment being added to the correct spot in the DOM! 🎉

#### Rendering the element with partials

Right now we're using a `<p>` tag for the comment, but we had a beautifully styled component for a comment with a lot more markup, CSS classes, nested elements, etc, already. Fortunately, we don't have to type all of the HTML for a comment right into the `$()` method, because we're 1) inside a `.js.erb` template, and 2) we have a partial that represents a comment already!

```js
var added_comment = $("<%= render 'comments/comment', comment: @comment %>");
```

If you try this and look at the response, you'll see our partial being rendered beautifully:

![](/assets/render-without-escape-js.png)

Unfortunately, you'll also see that it no longer works; if you try adding a comment, the DOM no longer updates, and you'll see errors in the JS console.

The issue is that our HTML contains a lot of characters that are not allowed within a JavaScript string without first being escaped, just as there are many characters that aren't allowed within a Ruby string without first being escaped — first and foremost, you can't have `"` within a double-quoted string without first escaping it with a `\`.

Oh no! Does that mean we can't use our partials after all, and we have to type in all the HTML ourselves, being careful to escape every illegal character? Thankfully, no — Rails includes a helper method called `escape_javascript()`. Give it your string, and it will return another string with all characters that JavaScript doesn't like nicely escaped:

```js
var added_comment = $("<%= escape_javascript(render 'comments/comment', comment: @comment) %>");
```

Now give it a try and look at the response:

![](/assets/render-without-escape-js.png)

And, it works again — and it looks great. If you want to, you can use an abbreviation for the `escape_javascript()` helper — `j()`:

```js
var added_comment = $("<%= j(render 'comments/comment', comment: @comment) %>");
```

I sorta like how clearly `escape_javascript()` reads, but the brevity of `j()` is nice too. Your call.

#### Clear the form input

One last detail — the previous comment stays in the form textarea, which is inconvenient. Let's clear it out:

```js
$("#<%= dom_id(@comment.photo) %>_new_comment_form #comment_body").val("");
```

The selectors I needed to do this happened to already be in the HTML due to how `form_with` works, but I could have added them if I needed them. Ultimately it's up to you to 1) add the selectors you need, 2) craft the JavaScript/jQuery you need to update your interface. There's no formula to follow, and it will be unique to your context.

#### Have some fun

We should really probably stop here, but since it's our first time Ajaxifying and we're drunk with power, let's have some fun. Maybe the comment should slide down instead of just appearing?

```js
var added_comment = $("<%= j(render @comment) %>");

added_comment.hide();

$("#<%= dom_id(@comment.photo) %>_new_comment_form").before(added_comment);

added_comment.slideDown();

$("#<%= dom_id(@comment.photo) %>_new_comment_form #comment_body").val("");
```

### Ajaxify update

Next, let's improve the edit comment experience.

 1. When the edit icon is clicked, let's replace the comment with an edit form, right in place.
 2. When that form is submitted, let's replace it with the updated comment, right in place.

The same old steps:

 - Switch the link/form from HTML to JS with `remote: true` on `link_to` or `local: false` on `form_with`.
 - Add `format.js` to the appropriate `respond_to` block.
 - Write a JS response template. This will usually involve:
   - Using or creating partials to represent the components being rendered via Ajax.
   - Adding top-level elements with `id=""` attributes to the partials, if they don't already have them.
   - Writing some jQuery to select an existing element in the DOM and insert near it, replace it, etc.

See if you can Ajaxify edit/update on your own.

### Other challenges

Explore the target to find other things to practice Ajax on:

 - Like/unlike
 - Follow/unfollow

## Conclusion

If you can successfully Ajaxify the CRUD operations above, you're in great shape to build snappy, modern web applications that meet the expectations of today's users. The benefit of using this approach is that we're still using most of our code — routes, controllers, models, even view templates (especially partials).

Even better, we're still conforming to our straightforward _mental model_ of RCAV+CRUD, which is straightforward for a small team, or even a single-person team, to iterate on. This is a fantastic approach to use, especially in the early days while finding product/market fit, and satisfies the needs of 95% of the applications I've built for myself or for clients.

On the other hand: for very complicated "single page" applications that really don't fit the RESTful, document/URL paradigm (think Google Sheets), this approach using [Rails' "Unobtrusive AJAX"](https://guides.rubyonrails.org/working_with_javascript_in_rails.html){:target="_blank"} may not be the best choice. For cases like that, we will have to consider breaking apart our application into a JSON API back-end, and the clients will have to consume the API and typically be developed by their own specialized team (iOS, Android, or React for web). This dramatically increases cost and reduces development velocity, but in some cases, we have no choice. (But only use this approach when you don't have a choice! Far too many teams choose it when their app really doesn't need it.)
