# Cookies

We've come a long way:

 - We learned HTML, and put static pages in the `public/` folder for visitors to browse.
 - We learned Ruby!
 - We learned how to generate dynamic responses on the fly when a _URL_ is visited.
 - We learned how to react to user input using the _params_ hash.

We're able to build some pretty useful applications! For example, [our translation service](https://form-intro.matchthetarget.com/){:target="_blank"} using Google's Translate API and Twilio's SMS API is pretty amazing.

Now let's learn about a third part of the HTTP request besides the _URL_ and the _params_: **cookies**. This is going to be the first tool in our kits that will allow us to _persist information between requests_.

As of now, with the HTML, Ruby, R→C→A→V, & `params` that we've learned, we don't have any way to _store_ information. After we send the "V" in R→C→A→V, we forget everything.

With the addition of cookies, our new target will be this:

[https://omnicalc-2-cookies.matchthetarget.com/](https://omnicalc-2-cookies.matchthetarget.com/){:target="_blank"}

Note that the calculators now remember the results of previous calculations. Handy! Let's learn how to achieve this.

## Cookies are placed in the visitor's browser

While looking at the [target](https://omnicalc-2-cookies.matchthetarget.com/){:target="_blank"} in Chrome, open your Developer Tools. Click the "Application" tab, then click "<i class="fas fa-cookie-bite"></i> Cookies" in the left sidebar, and then find `https://omnicalc-2-cookies.matchthetarget.com` in the list of domains.

You're looking at your cookie jar!

Here's the deal: each domain that you visit is allowed to store a list of key/value pairs — i.e., a `Hash` — on your computer. It varies a little by browser, but in general, each domain is allowed to store 50 key/value pairs, and take up no more than 4 kilobytes of space. Each of these key/value pairs is called a **cookie**.

Try filling out a few of the calculator forms in the target, and watch what happens in your cookie jar. You can see that the values you enter are being saved as cookies. You can delete an individual cookie by clicking on it and pressing <kbd>delete</kbd>, or you can clear all of the cookies for the domain by clicking the <i class="fas fa-ban"></i> icon at the top of the list. If you do so, the app "forgets" your calculations.

Try a few more calculations, [open a new tab](https://omnicalc-2-cookies.matchthetarget.com/){:target="_blank"}, and check the cookie jar there. Your cookies are all still stored, even in the new tab! You could come back tomorrow and the results of your calculations today would be waiting for you.

Now, one last thing: try visiting the target in a different browser, or from a different device. Your old calculations will not be there. Cookies are browser-specific; not computer-specific, network-specific, or person-specific.

## Cookies are sent back to the server with _every subsequent request_

So, when you click submit on any of the forms, you can see that the target, in addition to doing the calculation, also stores a cookie in your browser. We'll practice how to do this shortly — it's not really anything new; we [add a key/value pair](https://chapters.firstdraft.com/chapters/767#creating-hashes){:target="_blank"} to a `Hash` that Rails provides called `cookies`.

The key thing to wrap our heads around is: the user's browser sends back the entire `cookies` hash with _every subsequent request the user makes_. In other words, after we add a key/value pair to `cookies` for a particular user, every time that user visits us again, we'll see that key/value pair in the `cookies` hash (unless they clear their cookies).

So we get to treat `cookies` like it's a permanent, user-specific `Hash` that persists between and across R→C→A→Vs, even though it really is not.

Okay, enough theory; let's practice!

## /add

In the 


the target is storing information on the visitor's own computer, within the cookie jar in their browser, in a `Hash`.

Here's the key thing: when the visitor comes back the next day, **they send the cookies back with their request**.

You may have heard of browser cookies (or HTTP cookies) in the past; they've been in the news recently. 

 they've gained a rather nefarious reputation these days, since they're used for everything from ad targeting to 
