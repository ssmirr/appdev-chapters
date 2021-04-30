# jQuery

### Manipulating HTML elements

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

### jQuery

In 2021, plain vanilla JavaScript is pretty nice to use; but before, say, 2015, it was quite a different story. The language itself was missing lots of features that we Rubyists take for granted, there were lots of cross-browser inconsistencies, and life was hard.

Sound familiar? It was the same story with CSS; with CSS, developers came together to share their solutions to rough edges and cross-browser inconsistencies with frameworks like Bootstrap. In the JavaScript world, the go-to library to make up for JavaScript's shortcomings was jQuery.

In 2021, jQuery isn't absolutely essential to survival, the way it was for many years. Many developers have started to drop it and just use vanilla JavaScript, which does have some benefits in terms of performance/the amount of data users have to download.

However, I still use jQuery on almost every project and I recommend that you do too, because I'm not interested in dealing with the 1% of cases where there are still rough edges. So, let's go grab it. [Here's a convenient CDN](https://code.jquery.com/) we can use:

```html
<script src="https://code.jquery.com/jquery-3.6.0.min.js" integrity="sha256-/xUj+3OJU5yExlq6GSYGSHk7tPXikynS7ogEvDej/m4=" crossorigin="anonymous"></script>
```

I've already added the above `<script>` tag to [this solutions file](https://github.com/appdev-projects/javascript_intro/blob/b1bb82ae3b80fec5fc8ccc0cb4dcf7bb2b7fa31d/hello.html). You can download it and work in there if you like, or you could start a file from scratch to experiment with.

#### Add some elements

Let's get some HTML elements in our page to work with.

```html
<script src="https://code.jquery.com/jquery-3.6.0.min.js" integrity="sha256-/xUj+3OJU5yExlq6GSYGSHk7tPXikynS7ogEvDej/m4=" crossorigin="anonymous"></script>

<h1>Cities</h1>
<p id="best">Chicago</p>
<p>Atlanta</p>
<p>New York</p>
<p>San Francisco</p>
<p><a id="add" href="#">Add Los Angeles</a></p>

<div><a id="hide" href="https://m.wikipedia.org/">Click here to hide</a></div>
<div><a id="show" href="https://m.wikipedia.org/">Click here to show</a></div>
<div><a id="toggle" href="https://m.wikipedia.org/">Click here to toggle</a></div>
```
