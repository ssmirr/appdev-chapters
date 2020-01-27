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

[You can install it here](https://chrome.google.com/webstore/detail/jsonview/chklaanhfefbnpoihckbnefhakgolnmc?hl=en) if you want to.

### http.rb gem

We're going to be using the open-source http.rb library, or "gem" in Ruby-land, to fetch pages. To install the gem, run the following command at a terminal prompt:

```bash
gem install http
```

To read pages on the internet, we can now leverage the `HTTP` class:

```ruby
require "http"

my_page = HTTP.get("https://www.chicagobooth.edu")

puts my_page
```

### Google Geocoding API

 - Example URL:

    https://maps.googleapis.com/maps/api/geocode/json?address=1600+Amphitheatre+Parkway,+Mountain+View,+CA&key=AIzaSyA5qwIlcKjijP_Ptmv46mk4cCjuWhSzS78

 - [Full documentation](https://developers.google.com/maps/documentation/geocoding/start)

### Dark Sky API

 - Example URL:

    https://api.darksky.net/forecast/26f63e92c5006b5c493906e7953da893/37.8267,-122.4233

### Parsing JSON

To turn a `String` containing data formatted as JSON into _actual_ Ruby `Hash`es, `Array`s, `Integer`s, etc:

```
require "http"
require "json"

raw_data = HTTP.get("https://someapi.com/mydata.json")

parsed_data = JSON.parse(raw_data)
```

Now it's just [hashes](https://chapters.firstdraft.com/chapters/767) and [arrays](https://chapters.firstdraft.com/chapters/758), which you can iterate over as usual with [.each](https://chapters.firstdraft.com/chapters/765) if you need to! You're off to the races.