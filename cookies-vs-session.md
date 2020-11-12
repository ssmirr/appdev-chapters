# More ways of using cookies

Rails provides a few different ways to work with browser cookies, in addition to the `cookies` hash that [we've already read about](https://chapters.firstdraft.com/chapters/842){:target="_blank"}.

## Flash messages

One are the `:alert` and `:notice` options that you are allowed to pass to the `redirect_to` method:

```ruby
# In a controller action:

redirect_to("/photos", { :notice => "Photo deleted." })
```

This sets a cookie in the user's browser that expires after exactly one more request. To access the notice and alert cookies (if present), there are shortcut methods `notice` and `alert` available in all view templates:

```erb
<p><%= notice %></p>
```

Usually, we put the above code in the application layout file `app/views/layouts/application.html.erb`, since we want notice and alert cookies to, if present, be displayed on any page the user might be redirected to.

Typically `notice` is used to display "everything worked as intended" messages, and `alert` is used to display "uh oh something didn't work, please try again" messages. Collectively, these messages are known as **flash messages**, since they only appear for one page load and then disappear.

## Session

Rails provides another hash called `session` that works very similarly to `cookies`: we still `.store` and `.fetch` key/value pairs with it.

The differences between `session` and `cookies` are:

 - All of the key/value pairs in `session` are encoded into one long cookie. If you store things in `session` and then look in Chrome's developer tools, individual keys will not be visible (the way that they were with `cookies`). Instead, there will be one long, gibberish-looking cookie. All of the key/value pairs in `session` are combined and encoded into it.

    This is primarily for security. If we're storing something important (like the ID of the signed in user), we can't allow users to simply modify it by opening the Developer Tools. `session`, unlike `cookies`, is tamper-proof.
 - `session` expires when the user quits their browser.
 - `session` allows you to store more than just `String`s as values. You could, for example, store an `Array` containing the last 10 product IDs that a user looked at.
 - There's a method that you can use in any controller called `reset_session`, which will delete all the key/value pairs that you've added. This is a convenient way to "forget" anything you've stored about a user, especially when signing them out.

Usually, I use `cookies` while figuring things out (since I can read them in the Dev Tools which makes debugging easier). Then I switch to `session`.
