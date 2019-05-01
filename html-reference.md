# HTML Reference

## The very basics

### p — a paragraph

Most text content is comprised of paragraphs, and most of the web is text; therefore, `p` elements are the workhorse of the web. But don't overuse them! Try to use them only for actual paragraphs of text. (If you're just looking for a generic block container, then try `div` instead.)

Examples:

```html
<p>
  Not much to it.
</p>
```

Produces:

<p>
  Not much to it.
</p>

### h1, h2, h3, h4, h5, h6 — a heading for any occasion

HTML offers six levels of headings — `h1` through `h6`. Choose one based on the correct semantic level of hierarchy — is it the top-level heading of the page? the secondary heading? tertiary?

Don't choose based on the default style (size, weight) that the browser assigns — we're going to override those awful default styles anyway.

Examples:

```html
<h1>Top-most heading</h1>
<h2>Second-level heading</h2>
<!-- etc -->
<h6>The last level</h6>
```

Produces:

<h1>Top-most heading</h1>
<h2>Second-level heading</h2>
<!-- etc -->
<h6>The last level</h6>

### img — images

Examples:

```html
<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/86/Evolution_of_a_Tornado.jpg/1024px-Evolution_of_a_Tornado.jpg">
```

Produces:

<img src="https://upload.wikimedia.org/wikipedia/commons/thumb/8/86/Evolution_of_a_Tornado.jpg/1024px-Evolution_of_a_Tornado.jpg">

### a — a link

Examples:

```html
Click <a href="https://www.google.com/">here</a> to search.
```

Produces:

Click <a href="https://www.google.com/">here</a> to search.

If you want the link to open in a new tab, add the `target="_blank"` attribute:

Examples:

```html
Click <a href="https://www.google.com/" target="_blank">here</a> to search but not leave us.
```

Produces:

Click <a href="https://www.google.com/" target="_blank">here</a> to search but not leave us.

## Lists

### Unordered and ordered lists

For simple lists, you have two choices: `ul` (unordered) or `ol` (ordered) lists.

Use `ul` if the items have no particular order. Use `ol` if the items are ordered.

By default, the browser will use bullets before each item in a `ul` and will automatically number each item in an `ol` (starting with 1). We can and will overwrite these default styles; most of the time we don't want bullets at all. So choose `ul` or `ol` based on the _semantics_ of the list, not on bullets.

#### li — each item in the list

Nested within the `ul` or `ol` should come one or more `li` elements that contain the actual items in the list.

Examples:

```html
<ul>
  <li>Apple</li>
  <li>Banana</li>
  <li>Orange</li>
</ul>
```

Produces:

<ul>
  <li>Apple</li>
  <li>Banana</li>
  <li>Orange</li>
</ul>

```html
<ol>
  <li>Apple</li>
  <li>Banana</li>
  <li>Orange</li>
</ol>
```

Produces:

<ol>
  <li>Apple</li>
  <li>Banana</li>
  <li>Orange</li>
</ol>

### Definition lists

When you have a list of things along with a _label_ for each thing, a `dl` (definition list) might fit the bill. Each label goes in a `dt` (definition term), and each piece of data goes in a `dd` tag (definition description).

Examples:

```html
<dl>
  <dt>First name</dt>
  <dd>Raghu</dd>

  <dt>Last Name</dt>
  <dd>Betina</dd>

  <dt>Role</dt>
  <dd>Instructor</dd>
</dl>
```

Produces:

<dl>
  <dt>First name</dt>
  <dd>Raghu</dd>

  <dt>Last Name</dt>
  <dd>Betina</dd>

  <dt>Role</dt>
  <dd>Instructor</dd>
</dl>

## Tables

In HTML, the `<table>` element is used to represent two-dimensional, tabular data. A simple table looks like this:

```html
<table border="1">
  <tr>
    <td>First</td>
    <td>Last</td>
  </tr>
  <tr>
    <td>John</td>
    <td>Doe</td>
  </tr>
  <tr>
    <td>Jane</td>
    <td>Doe</td>
  </tr>
</table>
```

which would produce this[^border1]:

[^border1]: The `border="1"` attribute is rarely used. It throws a quick and dirty border around all cells; I include it here to make it easy to see what's going on in these examples. In reality, we would use CSS if we want borders.

<table border="1">
  <tr>
    <td>First</td>
    <td>Last</td>
  </tr>
  <tr>
    <td>John</td>
    <td>Doe</td>
  </tr>
  <tr>
    <td>Jane</td>
    <td>Doe</td>
  </tr>
</table>

The things to keep in mind about tables are:

 - Every piece of data must reside within a cell, a `<td>` (table data) element.
 - Every `<td>` element must reside within a row, a `<tr>` element.
 - Every `<tr>` must have the same number of cells, or things get out of whack.

Despite this last rule, you can, however, "merge" cells using the `colspan` attribute:

```html
<table border="1">
  <tr>
    <td colspan="2">People</td>
  </tr>
  <tr>
    <td>John</td>
    <td>Doe</td>
  </tr>
  <tr>
    <td>Jane</td>
    <td>Doe</td>
  </tr>
</table>
```

produces:

<table border="1">
  <tr>
    <td colspan="2">People</td>
  </tr>
  <tr>
    <td>John</td>
    <td>Doe</td>
  </tr>
  <tr>
    <td>Jane</td>
    <td>Doe</td>
  </tr>
</table>

You can see that we've merged two cells in the first row together; the `colspan="2"` on the first cell ensures that we don't violate the "every `<tr>` must have the same number of cells" rule.

There's also a similar `rowspan` attribute to merge vertically, although we use it much more rarely.

## Generic elements

### div — a generic block level container

### span — a generic in-line container

## Housekeeping

### html

### head

### body

#### title

#### meta

#### link
