# Omnicalc API

At present, this is a brand new, blank Rails application. The **specifications** (_specs_, for short) for the application — in other words, the URLs that we need to build — are below. The URLs will look like `/flexible/square_root/42`, and the page will display the square root of the number at the end.

The difference between these URLs and [the ones we've seen before](https://chapters.firstdraft.com/chapters/779) is that they are _flexible_, not literal. So rather than always going to URLs that we, the developers, make up in advance, users can visit URLs that they make up — as long as those URLs fit patterns that we allow. Then, we will be able to examine what the users typed in the URL and use that as we craft our response.

In other words, dynamic URLs allow for **user input**. Let's make them work, one at a time.

## Part I: Flexible Path Segments

### Square with flexible path segment

If I visit a URL of the pattern:

```
/flexible/square/[ANY INTEGER]
```

I should see the square of the number in the third segment of the path. I should be able to enter any **integer** in the third segment of the path; but not a decimal number (since if you have a dot in a URL, Rails will interpret what comes after it as a file extension/format).

#### Example

If I visit [http://[YOUR APP DOMAIN]/flexible/square/5](http://[YOUR APP DOMAIN]/flexible/square/5), I should see something like:

```
{"number":42,"square":1764}
```

Depending on what browser you're using or whether you're using something like [the JSONView Chrome Extension](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en), you might see something formatted more nicely like:

```json
{
  number: 42,
  square: 1764
}
```

To make a path segment **flexible**, start it off with a colon in your route:

```ruby
match("/flexible/square/:the_number", { :controller => "application", :action => "flex_square", :via => "get" })
```

Then, whatever value the user types in that segment of the path will be placed into a special `Hash` that Rails creates and names `params`. The value will be stored under the key `:the_number`, since that's what we labeled that segment in the route.

In other words, if you now visit http://[YOUR APP DOMAIN]/flexible/square/42, then you will be routed into the `flex_square` action and along with a `Hash` called `params` that looks like this:

```ruby
{ "the_number" => 42 }
```

After adding such a route and action, try visiting that URL and watch your server log as you do it.

Now, in our action (in this case, we named the method `flex_square`) we can `.fetch` the key `"the_number"` from the special `Hash` named `params` that Rails creates for us containing all user input. Usually we'll throw the value into a variable, do whatever processing we need on it, and then finally display some output to the user as usual using the `render()` method.

### Square Root with flexible path segment

If I visit a URL of the pattern:

```
/flexible/square_root/[ANY INTEGER]
```

I should see the square root of the integer in the third segment of the path.

#### Example

If I visit [http://[YOUR APP DOMAIN]/flexible/square_root/8](http://[YOUR APP DOMAIN]/flexible/square_root/8), I should see something like:

```json
{
  number: 8,
  square: 2.8284271247461903
}
```

### Random with flexible path segments

If I visit a URL of the pattern

```
/flexible/random/[MIN]/[MAX]
```

I should see a random number that falls between the integers in the third and fourth segments of the path.

#### Example

If I visit [http://[YOUR APP DOMAIN]/flexible/random/50/100](http://[YOUR APP DOMAIN]/flexible/random/50/100), I should see something like

```json
{
  min: 50,
  max: 100,
  random: 84
}
```

### Notes for flexible path segment tasks

**All of the above should work no matter what integers I type into the flexible segments of the path.**

Remember:

 - **Rails places all user input in the `params` hash.**
 - You can use the `params` hash in your actions as you would any `Hash`; `.keys`, `.fetch`, etc.
 - Watch the server log to see what the `params` hash contains for any given request.

#### Your task: Build out flexible RCAVs so that all of these (infinitely many) URLs work.

## Part II: Query Strings

Now, let's build something a little more realistic. Restricting to integer inputs is not very useful for most calculations — how would we allow decimal inputs? We've seens the technique before when we were using the Google Maps API — **query strings**. This is the part of the URL that, optionally, comes _after_ the path; it starts with a `?` and then has a list of key/value pairs. It's basically the URL version of a `Hash`; this:

```
?sport=hockey&dessert=cookies
```

is the query string version of this:

```ruby
{ :sport => "hockey", :dessert => "cookies" }
```

in Ruby. Try copying the above query string and pasting it onto the end of _any_ functional URL in your app, press return, and watch the server log as you do so. What do you observe?

The information ends up in the `params` hash automatically! No need to modify the route at all. Interesting...

Use that to build the following endpoints:

### Square

```
http://[YOUR APP DOMAIN]/square/results?input_number=42.42
```

#### Example output

```json
{
  number: 42.42,
  square: 1799.4564
}
```

### Square root

```
http://[YOUR APP DOMAIN]/square_root/results?input_number=8.3
```

#### Example output

```json
{
  number: 42.42,
  square: 2.8809720581775866
}
```

### Random

```
http://[YOUR APP DOMAIN]/random/results?input_min=3.14&input_max=9.99
```

#### Example output

```json
{
  min: 3.14,
  max: 9.99,
  random: 5.3403
}
```

### Monthly Payment

```
http://[YOUR APP DOMAIN]/payment/results?input_apr=5.42&input_years=30&input_pv=151250
```

#### Example output

```json
{
  purchase_price: 235000,
  percent_down: 3.5,
  apr: 4.35,
  years: 30,
  monthly_payment: 1128.91
}
```

Use the formula seen in the image located at `/public/payment_formula.gif` to calculate the monthly payment. Remember to keep your units straight — we want a **monthly** payment, but the interest rate and term are given in years.
