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

Don't choose based on the default style that the browser assigns — we're going to override those awful default styles anyway.

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

### a — a link

## Lists

### Unordered and ordered lists

#### ul, ol — the list itself

#### li — each element in the list

### Definition lists

#### dl — the list itself

#### dl, dt — each element in the list

## Tables

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
