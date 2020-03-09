# Day 4 Notes

## Getting started

 - Read about Gitpod and create a workspace based on the `appdev-projects/hello.rb` repository as described within:

    [https://chapters.firstdraft.com/chapters/785](https://chapters.firstdraft.com/chapters/785){:target="_blank"}

 - In your new workspace, create a file called `rock.rb` and write some Ruby in it; for example,
    
    ```ruby
    p "Hello, world!"
    ```
    
    Save the file.
 - In the terminal at the bottom of the workspace, after the `$`, enter:

    ```bash
    ruby rock.rb
    ```

    and press <kbd>return</kbd>. You should see something like:

    ```bash
    gitpod /workspace/hello.rb $ ruby rock.rb
    Hello, world!
    ```
  - Enhance the program to randomly choose a computer move between `rock`, `paper`, or `scissors` and then display the outcome accordingly (assuming the player played `rock`).
  - Repeat for `paper.rb` and `scissors.rb`.

## Exploring APIs

Let's get into one of my favorite things about programming: leveraging the power of APIs.

Create a file called `umbrella.rb`, and let's get to it.

### The http gem

We're going to be using an open-source library[^libraries] called `http` (which has [a nice wiki](https://github.com/httprb/http/wiki){:target="_blank"}, by the way) to fetch pages. To install the gem, run the following command at a terminal prompt:

[^libraries]: In Ruby-land, libraries are called "gems". In Python, "packages". Etc.

```bash
gem install http
```

To read pages on the internet, we can now leverage the `HTTP` class:

```ruby
require("http")

my_page = HTTP.get("https://www.imdb.com/movies-in-theaters/")

p my_page
```

`my_page` now contains a gigantic `String` which has the HTML source code of [imdb.com/movies-in-theaters](https://www.imdb.com/movies-in-theaters/){:target="_blank"} within it. We would have to then figure out a way to get the data that we want out of it — this is called "web scraping", and can be tricky.

Hopefully, if a company wants you to CRUD into/out of their database, they will instead expose the data in a machine-friendlier format: JSON. This parallel, machine-friendly version of an application is known as an API — Application Programming Interface.

### Dark Sky API

[Dark Sky](https://darksky.net/forecast/40.7127,-74.0059/us12/en){:target="_blank"} is a company that aggregates extremely fine-grained weather data. I use their iPhone app heavily, although I never open it, because it sends me a push notification whenever it's about to rain where I am (so that I can leave early).

Their iPhone app is mainly just an advertisement to their real product, which is their API. They sell access to their data to other developers.

 - Here's an example page in the Dark Sky API. Try it in a browser tab (it won't work until you replace the API key with my real key from Canvas):

    ```
    https://api.darksky.net/forecast/SECRET-API-KEY-CAN-BE-FOUND-ON-CANVAS/37.8267,-122.4233
    ```
 - [Full documentation](https://darksky.net/dev/docs){:target="_blank"}
  
### Google Geocoding API

 - Example URL:

    ```
    https://maps.googleapis.com/maps/api/geocode/json?address=1600+Amphitheatre+Parkway,+Mountain+View,+CA&key=SECRET-API-KEY-CAN-BE-FOUND-ON-CANVAS
    ```

 - [Full documentation](https://developers.google.com/maps/documentation/geocoding/start){:target="_blank"}

### JSONView

JSONView is a Chrome Extension that formats JSON nicely in the browser window, making it easier for us to fold/unfold and explore nested data structures.

[You can install it here](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en){:target="_blank"} if you want to.

### Parsing JSON

To turn a `String` containing data formatted as JSON into _actual_ Ruby `Hash`es, `Array`s, `Integer`s, etc:

```
require("http")
require("json")

raw_data = HTTP.get("https://someapi.com/mydata.json") # => Put your own API URL here.

p raw_data.class # => Ewwww, a String
p raw_data.length # => And it's huge

# Let's make that data more tractable by parsing it:
parsed_data = JSON.parse(raw_data)

p parsed_data.class # => Much better!
```

Now it's just [hashes](https://chapters.firstdraft.com/chapters/767) and [arrays](https://chapters.firstdraft.com/chapters/758) all the way down. You can use `.fetch`, `.at`, and [.each](https://chapters.firstdraft.com/chapters/765) if you need to! You're off to the races.

### Challenge

#### Part 1

**Goal:** Transform a street address into a latitude and longitude.

 - Put together the URL within the Google Geocoding API that contains the longitude and latitude of that street address.
 - Use `http.get()` to read the data at the URL you put together.
 - If you're getting an error having to do with an invalid request, there are most likely characters in your street address that are illegal to have inside URLs. You can substitute them with `gsub`, or the handy `URI.encode` method:

    ```ruby
    URI.encode("5807 S Woodlawn Ave") # => => "5807%20S%20Woodlawn%20Ave"
    ```
 - If you get stuck, use the `p` method to print A LOT. Like, every line if you need to. **Make the invisible visible.** It's helpful to label each piece of data before you display it (with another call to `p`rint) that you're printing so you know what you're looking at.
 - Use `JSON.parse()` to transform the raw `String` response into hashes and arrays.
 - Use the `.class`, `.keys` or `.fetch` (if you're dealing with a `Hash`), `.length` or `.at(0)` (if you're dealing with an `Array`), to drill down into the data until you find the latitude and longitude. 
    - Exploring the data at the API URL using the [JSONView extension](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en) might help.
 - Print the street address entered along with its latitude and longitude.
  
#### Part 2

**Goal:** Transform a latitude and longitude into the current temperature.

 - Take the latitude and longitude that you found in Part 1 and put together the URL within the Dark Sky API that contains the weather for that location.
 - Use `http.get()` to read the data at the URL you put together.
 - Use `JSON.parse()` to transform the raw `String` response into hashes and arrays.
 - Use the `.class`, `.keys` (if you're dealing with a `Hash`), `.length` or `.at(0)` (if you're dealing with an `Array`), to drill down into the data until you find the latitude and longitude. 
    - Exploring the data at the API URL using the [JSONView extension](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en) might help.
 - Print the current temperature at the location.

#### Make it actually dynamic

 - Make the street address dynamic with `gets`.
 - If you get it to work, and want to continue on to the stretch goal, you should comment out the `gets` and go back to hardcoding the address (e.g. "5807 S Woodlawn Ave"). This is so that you don't have to keep typing the address over and over as you run the program to test it. You can switch it back to being dynamic once you're done.

##### Stretch goal

Can you figure out how to print a message "You better take an umbrella with you!" _if_ there is at least a 50% chance probability of precipitation at that location within the next 12 hours?

Potentially useful things:

 - Find a location that has some precipitation in the next 12 hours to test with.
 - You can use the `Array` `.[]` method with a `Range` argument to slice out a section of an array:

    ```ruby
    a = ["alice", "bob", "carol", "doug", "ellen", "frank"]
    a[1..3] # => ["bob", "carol", "doug"]
    ```
 - You can use the `Time.at()` method to parse an `Integer` ([Unix epoch time](https://en.wikipedia.org/wiki/Unix_time){:target="_blank"} format) into a `Time`.
 - You can use `Time.now` to get the current time.
 - Times are, by default, shown in the `UTC` (Universal Coodinated Time, a.k.a. Greenwich Mean Time) timezone. Chicago is either five or six hours behind that (depending on daylight savings).

### Twilio

We can do more than just READ records from APIs; we can CREATE records too.

A company called Twilio specializes in delivering text messages and other telephony, so that you don't have to build all that infrastructure. You just have to place an order to send a text message by creating a record in their database. They will then deliver it and charge you.

I've already signed up for an account; you can use my API credentials (Account SID, Secret Token, and Sender Phone Number), which you'll find on Canvas.

A slight wrinkle is that Twilio uses a different authentication method than just plopping the API token right inside the URL. So, first, you have to do something like this:

```
browser = HTTP.basic_auth({ :user => "YOUR-ACCOUNT_SID", :pass => "YOUR-TOKEN" }) 
# We've stored the authenticated "browser" in this variable, and we can use it from now on.
```

This is the equivalent of signing in with our username and password (which Twilio wants to be our account SID and token).

A second wrinkle is that, rather than using `HTTP.get()`, we'll use `HTTP.post()`. The POST HTTP verb is used for creating a new, while GET is for reading a resource. Therefore, the `.post` method needs more info: what the attributes are of the record that you want to create (in this case, the message we want sent, and who the recipient should be). Imagine you're filling out a form on Twilio's website to create a new record in their Orders table and clicking submit.

The URL to POST to in order to create a new message looks like:

```
https://api.twilio.com/2010-04-01/Accounts/YOUR-ACCOUNT-SID/Messages.json
```

So, ultimately, we need to do:

```ruby
browser.post(messages_url, data_to_post)
```

Assuming that we've set up `messages_url` and `data_to_post` correctly, and authenticated.

Here's an example putting it all together:

```ruby
require("http")

# Creating some helper variables containing credentials to keep later lines short.
account_sid = "TWILIO-ACCOUNT-ID-CAN-BE-FOUND-ON-CANVAS"
token = "SECRET-API-KEY-CAN-BE-FOUND-ON-CANVAS"
sender_number = "SENDING-PHONE-NUMBER-CAN-BE-FOUND-ON-CANVAS"

# Putting together my API URL, as usual.
messages_url = "https://api.twilio.com/2010-04-01/Accounts/" + account_sid + "/Messages.json"

# This is the extra authentication step, as opposed to before.
browser = HTTP.basic_auth({ :user => account_sid, :pass => token }) 

# Here we assemble the data that we want to insert. Twilio wants it as a nested hash:
data_to_post = {
  :form => {
    "Body" => "You'd better take an umbrella!",
    "From" => sender_number,
    "To" => "+19876543210" # Put your own phone number here. Eventually you would put your users' phone numbers here, if you're sending notifications.
  }
}

# Finally, we call the .post method:
browser.post(messages_url, data_to_post)
```

Voilà! You should have received a text message (unless I've invalidated my API token, which I usually do a day or so after class is over). You can now use this code where you previously just `p`rinted "You should take an umbrella!"

Once I invalidate my token (usually a day or so after class), you'll have to sign up for your own account and get your own Twilio credentials. If you use [this referral link](www.twilio.com/referral/86ykDX){:target="_blank"} to sign up then you'll get $10 in credits after your first purchase (and so will our class experimentation pool of funds — thanks!).

### Scraping the web

Sometimes, the data that we want isn't offered through an API, and we have no choice but to pull it out of HTML. This is not great, and I wouldn't build a business by relying on the structure of any page's HTML, as it is quite brittle. But, it is often handy for e.g. assembling datasets. So here goes:

Let's say we want to find out what movies are in theaters next week so we can organize an outing (a real student project from the past).

 - Here's a page with that info: [https://www.imdb.com/movies-in-theaters/](https://www.imdb.com/movies-in-theaters/){:target="_blank"}
 - The problem is that the data isn't nice, neat JSON. It's a crazy pile of HTML. But, don't despair: we can still parse it, using the Nokogiri gem.
 - Set up your scraping program just like you did before — read the page with `HTTP.get()`. However, rather than `JSON.parse()`, use `Nokogiri::HTML.parse()`. In order to do so, you will need to:
    - At a terminal prompt, `gem install nokogiri`
    - In your program, `require("nokogiri")`
  - Nokogiri reads the source code and interprets it, the same way Chrome does, into a Document Object Model (DOM). It has the structure that you see when you right-click and Inspect an element in Chrome.
  - To dig elements out of the DOM, we are going to query it the way that we've learned before: CSS selectors! Pretend that you had to write a CSS rule to put a red border around the bits of data that you want. Your challenge is to come up with a selector that targets the data that you want, and only the data that you want, given that you have no control over the HTML. Then use the `.css` method.
  
     For example, if we want to target the h4s on the page:

    ```ruby
    matches = parsed_data.css("h4")
    ```

 - `matches` is now an Array-like object containing all of the matching elements - it can be accessed using `.at()`, or looped over using `.each`.
 - You can also use the `.text` element to just get all of the text within that element (including children).
 - You can also use `.css()` again on it to query further.
 - Basically, web scraping consists of:
    - **Coming up with the correct CSS selector** that matches the set of elements that has the info that you want, and only those elements.
    - Looping through the matches with `.each`.
    - Querying further if you need to pinpoint particular pieces of data in child elements with `.css()`.
    - Using `.text` (and usually `.strip`) to get the data out of the element.
    - Saving it into a database, producing a CSV, emailing it, or whatever.
  - Here are two things that you should do if you plan to scrape the web:
    - Get really good at CSS selectors by playing this game, which should take you around an hour: [CSS Diner](https://flukeout.github.io/){:target="_blank"}
    - Check out this browser extension: [Selector Gadget](https://selectorgadget.com/){:target="_blank"}

#### Filling out forms

If you actually need to fill out forms and things, then there's another gem that's built on top of Nokogiri: [Mechanize](https://github.com/sparklemotion/mechanize){:target="_blank"}.

#### Don't be a jerk

But be careful: many sites will block you if they notice you are scraping too much and putting too much stress on their servers.

The most basic thing to do is add a delay between scrapes, if you're running inside a loop. You can use the `sleep()` method to pause your code (the argument is seconds).

Here's [an article with more tips on avoiding being blocked](https://www.scrapingbee.com/blog/web-scraping-without-getting-blocked/){:target="_blank"}.
