# Cookies

We've come a long way:

 - We learned HTML, and put static pages in the `public/` folder for visitors to browse.
 - We learned Ruby!
 - We learned how to generate dynamic responses on the fly when a URL is visited.
 - We learned how to react to user input using the params hash.

We're able to build some pretty useful applications! For example, [our translation service](https://form-intro.matchthetarget.com/){:target="_blank"} using Google's Translate API and Twilio's SMS API is pretty amazing.

Now let's learn about a third part of the HTTP request besides the URL and the params: **cookies**. This is going to be the first tool in our kits that will allow us to _persist information between requests_.

As of now, with the HTML, Ruby, R→C→A→V, and `params` that we've learned, we don't have any way to _store_ information. After we send our users the "V" in R→C→A→V, we forget everything — all our instance variables, everything goes poof at the end of each action.

However! With the addition of cookies, our first storage mechanism, we'll be able to upgrade our little calculator to this, our new target:

[https:/cookies-intro.matchthetarget.com/](https:/cookies-intro.matchthetarget.com/){:target="_blank"}

Note that the calculators now remember the results of previous calculations. Handy! Let's learn how to achieve this.

## Cookies are placed in the visitor's browser

While looking at the [target](https:/cookies-intro.matchthetarget.com/){:target="_blank"} in Chrome, open your Developer Tools. Click the "Application" tab, then click "<i class="fas fa-cookie-bite"></i> Cookies" in the left sidebar, and then find `https:/cookies-intro.matchthetarget.com` in the list of domains.

You're looking at your cookie jar!

Here's the deal: each domain that you visit is allowed to store a list of key/value pairs — i.e., a `Hash` — on your computer. It varies a little by browser, but in general, each domain is allowed to store 50 key/value pairs, and take up no more than 4 kilobytes of space. Each of these key/value pairs is called a **cookie**.

Try filling out a few of the calculator forms in the target, and watch what happens in your cookie jar. You can see that the values you enter are being saved as cookies. You can delete an individual cookie by clicking on it and pressing <kbd>delete</kbd>, or you can clear all of the cookies for the domain by clicking the <i class="fas fa-ban"></i> icon at the top of the list. If you do so, the app "forgets" your calculations.

Try a few more calculations, [open a new tab](https:/cookies-intro.matchthetarget.com/){:target="_blank"}, and check the cookie jar there. Your cookies are all still stored, even in the new tab! You could come back tomorrow and the results of your calculations today would be waiting for you.

Now, one last thing: try visiting the target in a different browser, or from a different device. Your old calculations will not be there. Cookies are browser-specific; not computer-specific, network-specific, or person-specific.

## Cookies are sent back to the server with _every subsequent request_

So, when you click submit on any of the forms, you can see that the target, in addition to doing the calculation, also stores a cookie in your browser. We'll practice how to do this shortly — it's not really anything new; we [add a key/value pair](https://chapters.firstdraft.com/chapters/767#creating-hashes){:target="_blank"} to a `Hash` that Rails provides called `cookies`.

The key thing to wrap our heads around is: the user's browser sends back the entire `cookies` hash with _every subsequent request the user makes_. In other words, after we add a key/value pair to `cookies` for a particular user, every time that user visits us again, we'll see that key/value pair in the `cookies` hash (unless they clear their cookies).

So we get to treat `cookies` like it's a permanent, user-specific `Hash` that persists between and across R→C→A→Vs, even though it really is not.

Okay, enough theory; let's practice!

## Storing cookies

### Study the existing code

First, study the code for the two R→C→A→Vs that are collaborating to let visitors add two numbers together.

 - What URL is responsible for the addition form?
 - Visit the form and see!
 - Use the server log while visiting pages help identify which URLs/controllers/actions are being used.
 - Note the `params` that are coming in with each request.
 - Read the routes, actions, and view templates _in sequence_.
 - Do you understand every bit of code that is involved? Every route? Every instance variable? Every attribute in every form?

If not, ask questions!

Now that we're familiar with the code, if wanted to store the result of each calculation in the `cookies` hash _before_ showing the results, which line of code in which file should we do that on?

Good — now we just have to store the value in the `cookies` `Hash`!

### Refresh your Hash skills

We've been taking things _out_ of `Hash`es an awful lot, between APIs and `params`, but it's been a minute since we've put things _in_ to a `Hash`. Here's a quick refresher: the method to add key/value pairs to a `Hash` is `Hash#store`. Unlike `Hash#fetch` and `Array#push`, `Hash#store` requires _two_ arguments — the value that you want to add, as well the key to associate the value to:

```ruby
h = Hash.new

h.store(:color, "pink")
h.fetch(:color) # => "pink"
```

For more of a refresher, you might want to refresh your memory by [playing around with the REPLs in the Hash Chapter](https://chapters.firstdraft.com/chapters/767#creating-hashes){:target="_blank"}.

### The cookies hash

Fortunately, we don't have to deal directly with browser cookies ourselves, much like we didn't have to deal directly with query strings or dynamic path segments ourselves. Rails handles parsing all of the pieces of the HTTP request for us, thank goodness, and gives us lovely, easy to use Ruby objects like `params`.

Similarly, Rails provides a `Hash` called `cookies`, available in all controller actions and view templates; and we just get to use it. Unlike the `params` `Hash`, which we only get to `fetch` from, we can both `store` values in `cookies` and `fetch` them back out later. Go ahead, give it a try:

```
cookies.store(:most_recent_addition, @result)
```

Open your Developer Tools, find your app's Gitpod domain in the cookie jar, and then submit your addition form. Voila! Your first cookie!

## Retrieving cookies

Now, can you display the most recent addition result on the page that shows the form to add two numbers? Give it a try.

## Rinse and repeat

Tasks:

 - Display the most recent subtraction result on /muggle_subtract.
 - Display the most recent subtraction result on /muggle_multiply.
 - Display the most recent subtraction result on /muggle_divide.
 - Display the most recent subtraction result on /muggle_translate.

## Conclusion

There are a few more nuances to browser cookies that we could go into — setting expiration dates for specific cookies, encrypting them so that they aren't so easily visible in the Developer Tools, making them tamper-proof — but basically you've got the gist of it. It's a small `Hash` that we get to add key/value pairs to, and the user's browser will send it back to us with every subsequent request; it's like a set of permanent `params` for that user.

One consequence of being able to store information in individual user's browsers is that we can store _unique_ values there, which allows us to distinguish visitors from one another. This unlocks a whole world of possibilities — everything from A/B testing to analytics to ad targeting. Depending on your perspective, browser cookies are responsible for vastly improving the web experience or chilling privacy invasions.

As far as we're concerned, there's one key thing that cookies enable that we can't live without: sign-in and sign-out. Once we have user stored in our database table, we can store a cookie in their browser with their ID number. After that, whenever they visit our app, the very first thing we'll do is `cookies.fetch(:user_id)`, and based on that number, we'll personalize the entire experience for them.

