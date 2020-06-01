## Step 1

Copy and paste the following code into your View template.

```html
<div>
  <div id="map" style='width: 800px; height: 400px;'></div>
</div>

<script>
  function initMap() {
    var mapDiv = document.getElementById('map');

    var map = new google.maps.Map(mapDiv);

    var bounds = new google.maps.LatLngBounds();
    
    var infowindow = new google.maps.InfoWindow({
      content: "Click Content" 
    });
    
    var marker = new google.maps.Marker({
      position: {lat: 41.8381065, lng: -87.6512738}, 
      map: map,
      title: 'Hello World!',
      icon: 'http://maps.google.com/mapfiles/ms/icons/red-dot.png' 
    });
    
    marker.addListener('click', function() {
      infowindow.open(map, marker);
    });

  bounds.extend(new google.maps.LatLng(41.8381065, -87.6512738 ));

  map.fitBounds(bounds);
  };
  
</script>
<script src="https://maps.googleapis.com/maps/api/js?key=[YOUR_API_KEY]&callback=initMap"
async defer></script>
```

Make sure you replace `[YOUR_API_KEY]` with your actual API key.

Run the server and visit the page in the browser to make sure the map appears without errors.

## Step 2

Edit the code to be DYNAMIC and specific to your data!

```js
var infowindow = new google.maps.InfoWindow({
  content: "Click Content" 
});
```

Replace `“Click Content”` with a description of the place at the pin drop. You can use normal ERB tags even in this JavaScript code, since it’s inside a `.erb` file.

```js
var marker = new google.maps.Marker({
    position: {lat: 41.8381065, lng: -87.6512738}, 
    map: map,
    title: 'Hello World!',
    icon: 'http://maps.google.com/mapfiles/ms/icons/red-dot.png' 
});
```

Replace the value of `lat` , `lng` , and `title`.

You can change the value of `icon` to be a different color of pin if you change `red-dot` to `purple-dot`, `blue-dot`,`yellow-dot`, etc. 

You can also change the icon to an entirely different image (although the image needs to be small).

Don’t forget to update the latitude and longitude for the bounds of the map. Those values should be dynamic.

```js
bounds.extend(new google.maps.LatLng(41.8381065, -87.6512738 ));
```

## Step 3

Add multiple dropped pins to the map.

In order to make this work, we need to repeat the steps that are required for each pin.

Let’s say we have an Array with some data that we want to have dropped pins for.

The Array looks like:

```ruby
locations = [
  {
    "id" => 1,
    "lat" => 41.8381065,
    "lng" => -87.6512738,
    "name" => "House of Cakes",
    "address" => "6189 N Canfield Ave, Chicago, IL 60631, USA",
    "description" => "Straight up somebody's house. They bake cakes in their pajamas.",
  },
  {
    "id" => 2,
    "lat" => 41.9100984,
    "lng" => -87.6852883,
    "name" => "Handlebar",
    "address" => "2311 W North Ave, Chicago, IL 60647, USA",
    "description" => "A bar with handles. Very easy to pick up! #LiftWithYourBack",
  }
]
```

For **each** of those `Hash`’s we want to drop a pin. So we can use an `each` loop.

```erb
<% locations.each do |place| %>
```

Start the loop **after** the `var bounds = new google.maps.LatLngBounds();` and _before_ `map.fitBounds(bounds);`.

Next, modify our `infowindow` and `marker` code slightly, so that we create a unique variable for each element in the Array.

First modify the code in charge of providing content to a clicked pin.

```erb
// Make an info window for this place
var infowindow_<%= place.fetch("id") %> = new google.maps.InfoWindow({
    content: "<b><%= place.fetch("name") %></b>"
  });
```

Using an ERB tag we can even dynamically create variable names!

Then modify the code that creates the pin.

```erb
// Make marker for this place
var marker_<%= place.fetch("id") %> = new google.maps.Marker({
    position: {lat: <%= place.fetch("lat") %>, lng: <%= place.fetch("lng") %>},
    map: map,
    title: "<%= place.fetch("name") %>",
    icon: 'http://maps.google.com/mapfiles/ms/icons/green-dot.png'
  });

```

Modify the code that displays the pop-up when you click on a pin.

```erb
// Click to show info window
marker_<%= place.fetch("id") %>.addListener('click', function() {
    infowindow_<%= place.fetch("id") %>.open(map, marker_<%= place.fetch("id") %>);
  });

```

Finally, make sure the map zoom contains each dropped pin.

```erb
// Add this marker in map bounds
bounds.extend(new google.maps.LatLng(<%= place.fetch("lat") %>, <%= place.fetch("lng") %>));

```

The end result should look something like:

```erb
<script>
function initMap() {
  // initiate a new map
  var mapDiv = document.getElementById('map');
  var map = new google.maps.Map(mapDiv);

  // An empty bounds variable for seeting automatic zoom level (bounds of map)
  var bounds = new google.maps.LatLngBounds();

  <%  locations.each do |place|   %>

  // Make info window for this place
  var infowindow_<%= place.fetch("id") %> = new google.maps.InfoWindow({
    content: "<b><%= place.fetch("name") %></b>" 
  });

  // Make marker for this place
  var marker_<%= place.fetch("id") %> = new google.maps.Marker({
    position: {lat: <%= place.fetch("lat") %>, lng: <%= place.fetch("lng") %>},
    map: map,
    title: "<%= place.fetch("name") %>",
    icon: 'http://maps.google.com/mapfiles/ms/icons/<%= colors.sample %>-dot.png'
  });

  // Click to show info window
  marker_<%= place.fetch("id") %>.addListener('click', function() {
    infowindow_<%= place.fetch("id") %>.open(map, marker_<%= place.fetch("id") %>);
  });

  // Add this marker in map bounds
  bounds.extend(new google.maps.LatLng(<%= place.fetch("lat") %>, <%= place.fetch("lng") %>));

  <% end %>

  map.fitBounds(bounds);
};

</script>
```

With all of that done, you should be able to visit the page again and see two pins on the map.
