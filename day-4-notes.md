# Day 4 Notes

## Getting started

 - Read about Gitpod and create a workspace based on the `appdev-projects/hello.rb` repository as described within:

    https://chapters.firstdraft.com/chapters/785

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

### JSONView

JSONView is a Chrome Extension that formats JSON nicely in the browser window, making it easier for us to fold/unfold and explore nested data structures.

[You can install it here](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en){:target="_blank"} if you want to.

### http.rb gem

We're going to be using the open-source http.rb library, or "gem" in Ruby-land, to fetch pages. To install the gem, run the following command at a terminal prompt:

```bash
gem install http
```

To read pages on the internet, we can now leverage the `HTTP` class:

```ruby
require "http"

my_page = HTTP.get("https://www.imdb.com/movies-in-theaters/")

p my_page
```

`my_page` now contains a gigantic `String` which has the HTML source code of [imdb.com/movies-in-theaters](https://www.imdb.com/movies-in-theaters/){:target="_blank"} within it. We would have to then figure out a way to get the data that we want out of it — this is called "web scraping", and can be tricky.

Hopefully, if a company wants you to CRUD into/out of their database, they will instead expose the data in a machine-friendlier format: JSON. This parallel version of an application is known as an API.

### Google Geocoding API

 - Example URL:

    ```
    https://maps.googleapis.com/maps/api/geocode/json?address=1600+Amphitheatre+Parkway,+Mountain+View,+CA&key=SECRET-API-KEY-CAN-BE-FOUND-ON-CANVAS
    ```

 - [Full documentation](https://developers.google.com/maps/documentation/geocoding/start){:target="_blank"}

### Dark Sky API

 - Example URL:

    ```
    https://api.darksky.net/forecast/SECRET-API-KEY-CAN-BE-FOUND-ON-CANVAS/37.8267,-122.4233
    ```
 - [Full documentation](https://darksky.net/dev/docs){:target="_blank"}

### Parsing JSON

To turn a `String` containing data formatted as JSON into _actual_ Ruby `Hash`es, `Array`s, `Integer`s, etc:

```
require "http"
require "json"

raw_data = HTTP.get("https://someapi.com/mydata.json") # => Put your own API URL here.

p raw_data.class # => Ewwww, a String
p raw_data.length # => And it's huge

parsed_data = JSON.parse(raw_data)

p parsed_data.class # => Much better!
```

Now it's just [hashes](https://chapters.firstdraft.com/chapters/767) and [arrays](https://chapters.firstdraft.com/chapters/758). You can use `.fetch`, `.at`, and [.each](https://chapters.firstdraft.com/chapters/765) if you need to! You're off to the races.

### Challenge

#### Part 1

**Goal:** Transform a street address into a latitude and longitude.

 - Get a street address from the user (with `gets`).
 - Put together the URL within the Google Geocoding API that contains the longitude and latitude of that street address.
    - Assume that the street address is a real one, for now — we can handle invalid user input later.
 - Use `http.get()` to read the data at the URL you put together.
 - Use `JSON.parse()` to transform the raw `String` response into hashes and arrays.
 - Use the `.class`, `.keys` (if you're dealing with a `Hash`), `.length` or `.at(0)` (if you're dealing with an `Array`), to drill down into the data until you find the latitude and longitude. 
    - Exploring the data at the API URL using the [JSONView extension](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en) might help.
 - Print the street address that the user entered along with its latitude and longitude.
  
#### Part 2

**Goal:** Transform a latitude and longitude into the current temperature.

 - Take the latitude and longitude that you found in Part 1 and put together the URL within the Dark Sky API that contains the weather for that location.
 - Use `http.get()` to read the data at the URL you put together.
 - Use `JSON.parse()` to transform the raw `String` response into hashes and arrays.
 - Use the `.class`, `.keys` (if you're dealing with a `Hash`), `.length` or `.at(0)` (if you're dealing with an `Array`), to drill down into the data until you find the latitude and longitude. 
    - Exploring the data at the API URL using the [JSONView extension](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en) might help.
 - Print the current temperature at the location.

##### Stretch goal

Can you figure out how to print a message "You better take an umbrella with you!" _if_ there is at least a 50% chance probability of precipitation at that location within the next 12 hours?


### Twilio

We can do more than just READ records from APIs; we can CREATE records too.

A company called Twilio specializes in delivering text messages and other telephony, so that you don't have to build all that infrastructure. You just have to place an order to send a text message by creating a record in their database. They will then deliver it and charge you.

I've already signed up for an account; you can use my API credentials, which you'll find on Canvas. A slight wrinkle is that Twilio uses a different authentication method than just plopping the API token right inside the URL. Here's how it goes:

```ruby
require "http"

account_id = "TWILIO-ACCOUNT-ID-CAN-BE-FOUND-ON-CANVAS"
token = "SECRET-API-KEY-CAN-BE-FOUND-ON-CANVAS"
sender = "SENDING-PHONE-NUMBER-CAN-BE-FOUND-ON-CANVAS"

url = "https://api.twilio.com/2010-04-01/Accounts/#{account_sid}/Messages.json"

http = HTTP.basic_auth({ :user => account_sid, :pass => token })
options = {
  :form => {
    "Body" => "hi there httprb",
    "From" => sender,
    "To" => "+19876543210" # Put your own phone number here
  }
}

http.post(url, options)
```
