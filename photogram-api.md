# Photogram API

In this project, you will build routes that allow external users to Create, Read, Update, and Delete records in your database.

## Your target

You can see [a working example of how your application should function here](http://photogram-api.herokuapp.com/users).

## Review the available query methods

First, head to the `app/models` folder and look over the instance methods that are defined in each of the models. Compare them to the ones you defined in the `photogram-queries` project. How similar or different was your implementation to mine? Ask questions on Piazza.

Don't modify the methods; the automated tests rely on them being present and functional.

## Your tasks

Build the following endpoints in the same way that we did in the `msm-api` project. For each endpoint, simply render the result of one of the existing instance methods, converted to json using `.to_json`.

## /users

For example:

```
/users
```

should show a list of all users.

## /users/[ANY EXISTING USERNAME]

For example:

```
/users/Galen
```

should show the details of one user.

## /users/[ANY EXISTING USERNAME]/own_photos

For example:

```
/users/Galen/own_photos
```

should show a list of the user's own photos.

## /users/[ANY EXISTING USERNAME]/liked_photos

For example:

```
/users/Galen/liked_photos
```

should show a list of the photos the user has liked.

## /users/[ANY EXISTING USERNAME]/feed

For example:

```
/users/Galen/feed
```

should show a list of the photos posted by people the user is following.

## /users/[ANY EXISTING USERNAME]/discover

For example:

```
/users/Galen/discover
```

should show a list of photos liked by the people the user is following.

## /photos/[ANY EXISTING PHOTO ID]

For example:

```
/photos/628
```

should show the details of one photo.

## /photos/[ANY EXISTING PHOTO ID]/likes

For example:

```
/photos/628/likes
```

should show a list of the photo's likes.

## /photos/[ANY EXISTING PHOTO ID]/fans

For example:

```
/photos/628/fans
```

should show a list of the photo's fans.

## /insert_like_record?input_photo_id=[ANY EXISTING PHOTO ID]&input_user_id=[ANY EXISTING USER ID]

For example,

```
/insert_like_record?input_photo_id=628&input_user_id=120
```

should add a record to the likes table.

## /delete_like/[ANY EXISTING LIKE ID]

For example:

```
/delete_like/4223
```

should delete a record from the likes table.

## /photos/[ANY EXISTING PHOTO ID]/comments

For example:

```
/photos/628/comments
```

should show a list of the photo's comments.

## /insert_comment_record?input_photo_id=[ANY EXISTING PHOTO ID]&input_user_id=[ANY EXISTING USER ID]&input_body=[THE COMMENT BODY]

For example,

```
/insert_comment_record?input_photo_id=628&input_user_id=120&input_body=An insightful comment
```

should add a record to the comments table.

## /update_comment_record/[ANY EXISTING COMMENT ID]?input_body=[THE UPDATED COMMENT BODY]

For example:

```
/update_comment_record/4288?input_body=A better comment body
```

should update the comment.
