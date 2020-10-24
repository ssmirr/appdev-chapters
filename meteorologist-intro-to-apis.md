# Meteorologist (Intro to APIs)

## Introduction

In this project, you will practice building forms and working with Arrays and Hashes by pulling data from external services like Google Maps. You will build an application that, given a street address, tells the user the weather forecast.

In order to achieve this, our first task will be to exchange a **street address** for a **latitude/longitude pair** using Google's Geocoding API. Then, we'll exchange a **latitude/longitude pair** for a **weather forecast** using Dark Sky's API.

We will send a location in a remarkably flexible, "English-y" format, the kind of thing we are allowed to type into a Google Maps search (e.g., "the corner of 58th and Woodlawn"), and the Geocoding API will respond with an exact latitude and longitude (along with other things) in JSON format.

### Getting Started

Any time you are trying to develop a proof of concept of an app that needs external API data, the first step is researching the API and finding out if the information you need is available.

For us, that translates to: is there a URL that I can paste into my browser's address bar that will give back JSON (JavaScript Object Notation) that has the data I need? If so, we're good; Ruby makes the rest easy.

If you want to, you can install a Chrome Extension called JSONView that makes reading JSON easier:

[https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en){:target="_blank"}

This will indent the JSON nicely and allow you to fold and unfold nested elements.

### API Setup

We will provide you with an API key for Google Maps in the assignment page for Omnicalc 2 on Canvas. You'll need this key to complete the exercises below.

We'll also need to make sure your API key stays hidden, in case your project ever gets pushed to GitHub or another public repository. Unsavory types like to scrape GitHub for sensitive information like API keys and run up huge bills for compromised users.

To keep certain information out of our code, we use **environment variables**. See the [Storing credentials securely](https://chapters.firstdraft.com/chapters/792){:target="_blank"} chapter, and add our GMaps API key to your environment.

### Find an example

Now, we have to research the API and find the correct URL to paste into our address bar to get back the JSON that we want. Usually, we have to start at the API Documentation. For the Geocoding API, the full docs are here:

[https://developers.google.com/maps/documentation/geocoding/](https://developers.google.com/maps/documentation/geocoding/){:target="_blank"}

but, as usual with technical documentation, I usually skim the intro and then head straight for the examples:

[https://developers.google.com/maps/documentation/geocoding/start#geocoding-request-and-response-latitudelongitude-lookup](https://developers.google.com/maps/documentation/geocoding/start#geocoding-request-and-response-latitudelongitude-lookup)

The first example they give is:

[https://maps.googleapis.com/maps/api/geocode/json?address=1600+Amphitheatre+Parkway,+Mountain+View,+CA&key=YOUR_API_KEY](https://maps.googleapis.com/maps/api/geocode/json?address=1600+Amphitheatre+Parkway,+Mountain+View,+CA&key=YOUR_API_KEY){:target="_blank"}

Replace `YOUR_API_KEY` with the one provided on Canvas in the Omnicalc 2 assignment. Paste that URL into a Chrome tab; you should see something like this:

![](/assets/mapsjson.png)

(If your JSON looks like a wall of text instead of the nicely indented version above, then you haven't installed or enabled the [JSONView Chrome extension](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en){:target="_blank"}.)

Try folding away the `address_components` section to make the value of the `geometry` key stand out, since that is where our target information lives:

![](/assets/folded-mapsjson.png)

Notice the `lat` and `lng` keys within the `location` hash. Notice that JSON uses curly braces for Hashes and square brackets for Arrays just like Ruby does.

Alright, now that we have the data we need showing up in the browser window, what's next? First of all, we should be sure that we can customize this example request to get data that we care about. So how would we get the coordinates of "5807 S Woodlawn Ave" instead of Google's HQ? Give it a try.

---

No really, try it yourself!

---

You'll have to modify the example URL somehow.

---

It turns out we need to replace the part of the URL after the `?address=` with the address we want geocoded:

![](/assets/modified-address.png)

(Spaces are not legal in URLs, so we have to **encode** them. One way to encode them can be seen in Google's example, with `+`s. If you tried typing the address with spaces in it, you'll have noticed that Google encodes spaces automatically with `%20%`.)

### Ruby

Great! Now we know the exact data we want is available through the API. Now, how do we get it into our application? Fortunately, Ruby comes with some powerful built-in functionality that will help us with this. First, we need Ruby to read the URL that has the JSON we want, just like Chrome did. Chrome has an address bar we can paste the URL in to; how can we tell Ruby to go open a page on the Internet?

In your Omnicalc 2 workspace, open a new terminal tab and then enter the command `rails console`. This launches an interactive Ruby sandbox for us to experiment in.

In the Rails Console, let's use Ruby's `open()` method to read Google's page. The `open()` method takes one `String` argument, which should contain the URL of the page you want to open. I'm going to copy-paste the URL within `""` and store it in a variable `url` to make it easier to work with:

```ruby
url = "https://maps.googleapis.com/maps/api/geocode/json?address=5807+S+Woodlawn+Ave&key="
  + ENV.fetch("GOOGLE_MAPS_KEY")
```

Then, let's `open` that URL and `read` the body of the page:

```ruby
open(url).read
```

You should see something like this:

![](/assets/open-url-read.jpg)

> **Note:** To scroll through long output in `rails console`, you can use <kbd>return</kbd> to scroll one line at a time, <kbd>Space</kbd> to scroll one page at a time, or <kbd>Q</kbd> to just get back to the prompt to enter a new Ruby expression.

What just happened? We `open`ed the page at the location in `url`, and the return value was an instance of the `StringIO` class, which represents the file. All we really want is the body of the file, the stuff that shows up in the browser window, so we used the `.read` method to pull that out as an instance of `String`. However, we just dropped that string on the ground; let's instead store the result in a variable called `raw_data`:

```ruby
raw_data = open(url).read
```

Alright! We just used Ruby to open up a connection over the Internet to Google's servers, placed a request for them to translate our address into a latitude and longitude, received a response, and stored it in a Ruby variable! That's a big deal, folks. However, the response is hideous. How in the world are we going to pull out the latitude and longitude values from that thing?

We could explore the [String](https://chapters.firstdraft.com/chapters/757){:target="_blank"} methods and find something that might help us scan through `raw_data` for `"lat"`, perhaps. But then what? We could probably figure it out, but there's a much better way.

Fortunately, Ruby provides a class called `JSON`, similar to the `CSV` class, which makes parsing a string that has data in JSON format a snap:

```ruby
parsed_data = JSON.parse(raw_data)                                                           
```

and you should see

```ruby
=> {"results"=>
  [{"address_components"=>
     [{"long_name"=>"Chicago Booth Harper Center",
       "short_name"=>"Chicago Booth Harper Center",
       "types"=>["premise"]},
      {"long_name"=>"5807", "short_name"=>"5807", "types"=>["street_number"]},
      {"long_name"=>"South Woodlawn Avenue", "short_name"=>"S Woodlawn Ave", "types"=>["route"]},
      {"long_name"=>"South Side", "short_name"=>"South Side", "types"=>["neighborhood", "political"]},
      {"long_name"=>"Chicago", "short_name"=>"Chicago", "types"=>["locality", "political"]},
      {"long_name"=>"Cook County",
       "short_name"=>"Cook County",
       "types"=>["administrative_area_level_2", "political"]},
      {"long_name"=>"Illinois", "short_name"=>"IL", "types"=>["administrative_area_level_1", "political"]},
      {"long_name"=>"United States", "short_name"=>"US", "types"=>["country", "political"]},
      {"long_name"=>"60637", "short_name"=>"60637", "types"=>["postal_code"]},
      {"long_name"=>"1610", "short_name"=>"1610", "types"=>["postal_code_suffix"]}],
    "formatted_address"=>"Chicago Booth Harper Center, 5807 S Woodlawn Ave, Chicago, IL 60637, USA",
    "geometry"=>
     {"bounds"=>
       {"northeast"=>{"lat"=>41.789511, "lng"=>-87.5949885},
        "southwest"=>{"lat"=>41.7883316, "lng"=>-87.5961513}},
      "location"=>{"lat"=>41.7891369, "lng"=>-87.5954551},
      "location_type"=>"ROOFTOP",
      "viewport"=>
       {"northeast"=>{"lat"=>41.79027028029149, "lng"=>-87.59422091970849},
        "southwest"=>{"lat"=>41.7875723197085, "lng"=>-87.59691888029151}}},
    "place_id"=>"ChIJkb9VkxYpDogRSmVtpZC9s6s",
    "types"=>["premise"]}],
 "status"=>"OK"}
```

Look! Hash rockets! We've converted a cumbersome string into a beautiful, friendly Ruby Hash. Now, if I want to get down to the latitude and longitude, how would I do it? Well, remember, we already got a hint back when we first looked at the data in Chrome: it has something to do with the `geometry` key. We can remind ourselves what keys are in the hash:

```ruby
parsed_data.keys
=> ["results", "status"]
```

The data we want is probably inside the key "results". So lets step in one level deep and see what we have:

```ruby
parsed_data.fetch("results").class
=> Array
```

Let's go one level deeper by getting the first element and seeing what _it_ is:

> **Note:** As you explore, don't forget that you can use your UP ARROW to scroll through your command line history. You don't always have to re-type the same thing over and over!

```ruby
f = parsed_data.fetch("results").at(0)
f.class
=> Hash
```

Another hash — let's see what keys it has:

```ruby
f.keys
=> ["address_components", "formatted_address", "geometry", "place_id", "types"]
```

Alright, so we now have a Hash in `f` that has a key called `"geometry"`, which, as we learned from our initial research above, is what contains our target. Let's keep going:

```ruby
f.fetch("geometry")
=> {"bounds"=>
  {"northeast"=>{"lat"=>41.789511, "lng"=>-87.5949885},
   "southwest"=>{"lat"=>41.7883316, "lng"=>-87.5961513}},
 "location"=>{"lat"=>41.7891369, "lng"=>-87.5954551},
 "location_type"=>"ROOFTOP",
 "viewport"=>
  {"northeast"=>{"lat"=>41.79027028029149, "lng"=>-87.59422091970849},
   "southwest"=>{"lat"=>41.7875723197085, "lng"=>-87.59691888029151}}}
```

Closer!

```ruby
f.fetch("geometry").fetch("location")
=> {"lat"=>41.7891369, "lng"=>-87.5954551}
```

Almost there!

```ruby
f.fetch("geometry").fetch("location").fetch("lat")
=> 41.7891369
f.fetch("geometry").fetch("location").fetch("lng")
=> -87.5954551
```

Woo! We made it all the way down to what we want. Phew! Now, I did it in a bunch of tiny steps, which might have made it seem complicated, but we could also have just done it in one step:

```ruby
parsed_data.fetch("results").at(0).fetch("geometry").fetch("location").fetch("lng")
=> -87.5954551
```

But, as you know, I much prefer making nicely named variables at each step, for clarity:

```ruby
url = "https://maps.googleapis.com/maps/api/geocode/json?address=5807+S+Woodlawn+Ave&key=" + ENV.fetch("GOOGLE_MAPS_KEY")
raw_data = open(url).read
parsed_data = JSON.parse(raw_data)
results_array = parsed_data.fetch("results")
first_result = results_array.at(0)
geometry_hash = first_result.fetch("geometry")
location_hash = geometry_hash.fetch("location")
latitude = location_hash.fetch("lat")
longitude = location_hash.fetch("lng")
```

And now I can do whatever interesting things with `latitude` and `longitude` that I need.

Now that we've explored in the console, it's time to take this Ruby (or something similar) and write some actions...

## Part 1: Street to Coordinatess

Now that we've seen how to retrieve information from a URL using Ruby, let's plug it in to a real application. If you haven't already, launch your Rails app with `bin/server` and navigate to the homepage in a Chrome tab.

Given what you've learned about wiring up forms, and now what you've learned above about reading from the Google Geocoding API, implement the [Street to Coordinates](https://omnicalc-2.matchthetarget.com/street_to_coords/new) functionality that you see in the target.

 - The URL `/street_to_coords/results` should display the lat/lon, provided a query string on the end that contains the key `user_street_address` and a value.
 - A form that makes it easy to arrive at the above URL with a query string should be available at `/street_to_coords/new`

If I type in `5807 S Woodlawn Ave` at the Street to Coordinates form, I should see something like:

<blockquote>
<dl>
  <dt>Street Address</dt>
  <dd>5807 S Woodlawn Ave</dd>

  <dt>Latitude</dt>
  <dd>41.7891387</dd>

  <dt>Longitude</dt>
  <dd>-87.5954555</dd>
</dl>
</blockquote>

## Part 2: Coordinates to Weather

Next, you will do something similar; but instead of using Google's Geocoding API, you will use the Dark Sky API. We will exchange a latitude/longitude pair for weather information. Dark Sky is an amazing API that gives you minute-by-minute meteorological information.

They released an iOS app, Dark Sky, to demonstrate the power of their API, and it instantly became a smash hit. The API is not entirely free, but we get 1,000 calls per day to play around with.

Step 1 when working with any API is research. What is the URL of the page that has the data we want, preferably in JSON format? Here is an example from their documentation:

[https://api.darksky.net/forecast/YOUR_API_KEY/37.8267,-122.4233](https://api.darksky.net/forecast/YOUR_API_KEY/37.8267,-122.4233){:target="_blank"}

The Dark Sky API is unfortunately no longer available for new signups, since Apple purchased them recently. You can use my key from Canvas. Here is what their dashboard looks like:

![](/assets/dark-sky-dashboard.png)

#### Find an example

Visit the example link and check out the data they provide. Scroll up and down and get a feel for it:

![](/assets/dark-sky-sample.png)

It's pretty amazingly detailed data; it tells us current conditions, along with minute-by-minute conditions for the next hour, hour-by-hour conditions for the next day or so, etc.

But first, can we customize the example to get data relevant to us? Plug in some coordinates that you fetched in `street_to_coords` and try it out.

Your job is to implement the actions required for the [Coordinates to Weather feature in the target](https://omnicalc-2.matchthetarget.com/coords_to_weather/new){:target="_blank"}.

<blockquote>
<dl>
  <dt>Latitude</dt>
  <dd>41.78</dd>

  <dt>Longitude</dt>
  <dd>-87.59</dd>

  <dt>Current Temperature</dt>
  <dd>73.35</dd>

  <dt>Current Summary</dt>
  <dd>Clear</dd>

  <dt>Outlook for next sixty minutes</dt>
  <dd>Clear for the hour.</dd>

  <dt>Outlook for next several hours</dt>
  <dd>Partly cloudy tomorrow morning.</dd>

  <dt>Outlook for next several days</dt>
  <dd>No precipitation throughout the week, with temperatures falling to 62°F on Tuesday.</dd>
</dl>
</blockquote>

**Note:** Forecast does not have data for every lat/lng combination; some geographies will return `nil`s. If you run into issues, using `dig` might be helpful, since it doesn't throw an error if a key is missing along the way:

```ruby
parsed_results.dig("minutely", "summary")
```

rather than

```ruby
parsed_results.fetch("minutely").fetch("summary")
```

[Read more about `.dig` here](https://tiagoamaro.com.br/2016/08/27/ruby-2-3-dig/){:target="_blank"}.

## Part 3: Address to Weather

Finally, pull it all together. Use both the Google Geocoding API and the Forecast API so that if I type in `5807 S Woodlawn Ave` at the Street to Weather form, I should see something like

<blockquote>
<p>Here's the outlook for 5807 S Woodlawn Ave:</p>

<dl>
  <dt>Current Temperature</dt>
  <dd>73.35</dd>

  <dt>Current Summary</dt>
  <dd>Clear</dd>

  <dt>Outlook for next sixty minutes</dt>
  <dd>Clear for the hour.</dd>

  <dt>Outlook for next several hours</dt>
  <dd>Partly cloudy tomorrow morning.</dd>

  <dt>Outlook for next several days</dt>
  <dd>No precipitation throughout the week, with temperatures falling to 62°F on Tuesday.</dd>
</dl>
</blockquote>

## Optional Extra Exercises, for fun

### Explore APIs (Easier)

Browse some APIs:

[https://chapters.firstdraft.com/chapters/800](https://chapters.firstdraft.com/chapters/800){:target="_blank"}

and get inspired!

### Bootstrap (Easier)

`<link>` to Bootstrap or a [Bootswatch](https://www.bootstrapcdn.com/bootswatch/){:target="_blank"} in the `<head>` of your pages (located in `app/views/layouts/application.html.erb`), and make things look prettier.

You can take [this Bootswatched app](https://omnicalc-bs4-final.herokuapp.com/){:target="_blank"} as inspiration, or create something entirely new.

### Future Forecast (Harder)

The Forecast API can take a third parameter in the URL, time:

https://developer.forecast.io/docs/v2#time_call

Add a feature that shows summaries for the next 14 days.

### Google Map (Harder)

Embed a Google map in the view, centered on the provided address. Refer to this chapter:

[https://chapters.firstdraft.com/chapters/836](https://chapters.firstdraft.com/chapters/836){:target="blank"}

The key concept is, just like with Bootstrap, to first paste in the example markup and see if it works.

Then, replace whichever part of the static markup you want to with embedded Ruby tags that contain your dynamic values.
