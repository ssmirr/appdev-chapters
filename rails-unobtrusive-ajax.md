# Rails Unobtrusive Ajax

## Objective

Now that we've learned [a minimal amount of JavaScript](https://chapters.firstdraft.com/chapters/861), let's apply our skills to our Rails applications to make them _slick_.

These notes are a companion to the [pg-ajax-1 project](https://github.com/appdev-projects/pg-ajax-1).

[This is our target.](https://pg-ajax-1.matchthetarget.com/)

## Comments

Let's focus first on the experience of CRUDing comments. Right now if you add, update, or destroy a comment, the `comments#create`, `comments#update`, and `comments#destroy` actions use the `redirect_back` method to send you back to the page you were previously on; which is pretty cool, actually. Much better than `redirect_to root_url` no matter where you were before, for example.


