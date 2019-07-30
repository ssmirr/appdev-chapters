# Very Best API

![](/assets/very-best-api-erd.png)

## Query helper methods to define

### User#bookmarks

### User#bookmarked_dishes

### User#bookmarked_venues

### User#bookmarked_neighborhoods

### Venue#neighborhood

### Venue#bookmarks

### Venue#specialties

### Venue#fans

### Neighborhood#venues

### Dish#bookmarks

### Dish#cuisine

### Dish#specialists

### Dish#fans

### Cuisine#dishes

### Cuisine#specialists

### Bookmark#dish

### Bookmark#venue

### Bookmark#user

## API Endpoints to build

### /cuisines

List all cuisines.

### /dishes

List all dishes.

### /dishes?cuisine_id=1

If a query string is present with a cuisine ID, use it to filter the list of dishes.

Be careful not to break the URL you built previously, plain old `/dishes` (without a query string on the end)! Requests both with and without a query string should be handled gracefully. There are [many](https://chapters.firstdraft.com/chapters/763) different ways you can achieve this.

### /dishes/[ANY EXISTING DISH ID]

Show the details of one dish.

/neighborhoods

/venues

/venues?neighborhood_id=1

/venues/[ANY EXISTING VENUE ID]

/users

/users/[ANY EXISTING USER ID]

/users/[ANY EXISTING USERNAME]

/add_bookmark?user_id=1&dish_id=2&venue_id=3

/users/[ANY EXISTING USER ID]/bookmarks

/dishes/[ANY EXISTING DISH ID]/bookmarks

/venues/[ANY EXISTING VENUE ID]/bookmarks

/remove_bookmark/[ANY EXISTING BOOKMARK ID]

/users/[ANY EXISTING USER ID]/bookmarked_dishes

/users/[ANY EXISTING USER ID]/bookmarked_dishes?cuisine=Breakfast

/users/[ANY EXISTING USER ID]/bookmarked_venues?neighborhood=Humboldt Park

/dishes/[ANY EXISTING DISH ID]/specialists

/dishes/[ANY EXISTING DISH ID]/specialists?neighborhood=Humboldt Park

/venues/[ANY EXISTING VENUE ID]/specialties

/venues/[ANY EXISTING VENUE ID]/fans
