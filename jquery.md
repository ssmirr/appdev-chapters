# jQuery

## Manipulating HTML elements

JavaScript is okay, as far as languages go, but we were perfectly happy with Ruby. The only reason we're interested in JavaScript is that it has a unique ability: it can run inside the browser, and that means we can use it to modify HTML elements in our pages _without needing to refresh the page_. This is the key to making our apps feel like _apps_ and not pages.

Let's see how this works. In Chrome, open the JavaScript Console from the View > Developer menu. There, you have a REPL, much like `irb`, where you can type any JavaScript expression. (There's also great autocomplete functionality in the Chrome dev tools, so use tab often.)

Importantly, there is an object available, `document`, that represents the page itself. Next, let's try:

```js
let headings = document.getElementsByTagName("h1")
```

We've created a variable, `headings`, and stored an array of all the `h1` elements on the page within it. Neat!

As you were typing, you might have noticed the autocomplete showing you other methods that were available — `getElementsByClassName`, `getElementByID`, etc. These are all ways that we can query the DOM (Document Object Model) and find HTML elements; exactly the way we did when we were writing CSS selectors.

Let's keep going and get just one heading out of the array:

```js
let h = headings[0]
```

Now that we have one element in the variable `h`, we can do things like:

 -  `h.innerText` (will return the text content of the element)
 -  `h.classList.add("display-1")` (adds a CSS class to the element)
 -  `h.remove()` (removes the element from the DOM)
 -  and a whole bunch more. Neat!

## jQuery

In 2021, plain vanilla JavaScript is pretty nice to use; but before, say, 2015, it was quite a different story. The language itself was missing lots of features that we Rubyists take for granted, there were lots of cross-browser inconsistencies, and life was hard.

Sound familiar? It was the same story with CSS; with CSS, developers came together to share their solutions to rough edges and cross-browser inconsistencies with frameworks like Bootstrap. In the JavaScript world, the go-to library to make up for JavaScript's shortcomings was jQuery.

In 2021, jQuery isn't absolutely essential to survival, the way it was for many years. Many developers have started to drop it and just use vanilla JavaScript, which does have some benefits in terms of performance/the amount of data users have to download.

However, I still use jQuery on almost every project and I recommend that you do too, because I'm not interested in dealing with the 1% of cases where there are still rough edges. Let's create a new HTML file and start to experiment with jQuery. [Here's a convenient CDN](https://code.jquery.com/) we can use to pull it in:

```html
<script src="https://code.jquery.com/jquery-3.6.0.min.js" integrity="sha256-/xUj+3OJU5yExlq6GSYGSHk7tPXikynS7ogEvDej/m4=" crossorigin="anonymous"></script>
```

## Add some elements

Let's get some HTML elements in our page to work with. (You can add the standard HTML boilerplate around it if you want to.)

```html
<script src="https://code.jquery.com/jquery-3.6.0.min.js" integrity="sha256-/xUj+3OJU5yExlq6GSYGSHk7tPXikynS7ogEvDej/m4=" crossorigin="anonymous"></script>

<h1>Cities</h1>

<ul>
  <li class="northeast">New York</li>
  <li class="west">Los Angeles</li>
  <li id="best" class="midwest">Chicago</li>
  <li class="south">Houston</li>
  <li class="west">Phoenix</li>
</ul>
```

## The jQuery function

Our primary entrypoint into the utility of the library is one function: `jQuery()`. Let me point out right away the [the official jQuery documentation](https://api.jquery.com/jquery/) is quite good, so you should refer to it often.

The `jQuery()` function is sort of like the `document.getElementsByTag()`, `document.getElementsByClassName()`, `document.getElementByID()`, etc, functions that we met earlier — but all wrapped into one. You pass in any CSS selector (as a string) as an argument, and `jQuery()` will return an array of matching elements. Try this in the JS console:

```js
jQuery("li");
jQuery(".west");
jQuery("#best");
```

Since we're going to be using the `jQuery()` function _a lot_, they conveniently aliased it as `$()`, to save us a lot of typing. So the above could also be shortened to:

```js
$("li");
$(".west");
$("#best");
```

## DOM manipulation

Once we've queried the DOM for the elements we're interested in, jQuery provides many methods for manipulating them. Let's try putting an element in a variable:

```js
let x = $("#best");
```

Now, we can manipulate it:

```js
x.hide();
```

Poof! Let's bring it back:

```js
x.show();
```

Or, if we're not sure what state it's in (visible or invisible) to start with, we can toggle:

```js
x.toggle();
x.toggle();
x.toggle();
```

That's pretty abrupt, though. How about something a little smoother?

```js
x.hide();
x.fadeIn();
x.fadeOut();
x.fadeToggle();
```

Or even smoooooother:

```js
x.fadeToggle(5000);
```

Read about and experiment with the following methods. If you need to, refresh the page to reset the state of the elements:

 - [https://api.jquery.com/hide/](https://api.jquery.com/hide/)
 - [https://api.jquery.com/show/](https://api.jquery.com/show/)
 - [https://api.jquery.com/toggle/](https://api.jquery.com/toggle/)
 - [https://api.jquery.com/remove/](https://api.jquery.com/remove/)
 - [https://api.jquery.com/append/](https://api.jquery.com/append/)
 - [https://api.jquery.com/prepend/](https://api.jquery.com/prepend/)
 - [https://api.jquery.com/slideUp/](https://api.jquery.com/slideUp/)
 - [https://api.jquery.com/slideDown/](https://api.jquery.com/slideDown/)
 - [https://api.jquery.com/fadeToggle/](https://api.jquery.com/fadeToggle/)
 - [https://api.jquery.com/replacewith/](https://api.jquery.com/replacewith/)
 - [https://api.jquery.com/html/](https://api.jquery.com/html/)
