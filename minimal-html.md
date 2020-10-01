# A minimal introduction to HTML

The World Wide Web (the Web) is, fundamentally, a collection of documents that are identified by URLs (uniform resource locators)^[http].

^[http]: And transferred via the Hypertext Transfer Protocol (HTTP).

Documents on the Web can be in any format — CSV, PDF, JSON, DOCX, XLS, JPG, etc — as long as they are identified by a URL. But a very common format, and one particularly suited to the Web, is **HTML: Hypertext Markup Language**.

**Hypertext** is text that contains **hyperlinks** — references to other documents (including their URLs). Users can follow these hyperlinks to navigate from document to document.

Hypertext Markup Language (HTML) is how we embed hyperlinks within hypertext. For example, here is **plain text**:

```txt
English scientist Tim Berners-Lee invented the World Wide Web in 1989.
```

And here is some Hypertext Markup Language (HTML):

```html
English scientist <a href="https://en.wikipedia.org/wiki/Tim_Berners-Lee">Tim Berners-Lee</a> invented the <a href="https://en.wikipedia.org/wiki/World_Wide_Web">World Wide Web</a> in 1989.
```

In this example, we have "marked up" or "tagged" some of the text, `Tim Berners-Lee` and `World Wide Web`, as _anchors_ that lead to other documents.

 - Collectively, `<a href="https://en.wikipedia.org/wiki/Tim_Berners-Lee">Tim Berners-Lee</a>` is one **element**. Specifically, it is an _anchor_ element.
 - `<a>` is the **opening tag** of the element. The `a` is what makes it an anchor.
    - `href="https://en.wikipedia.org/wiki/Tim_Berners-Lee"` is an **attribute** of the element.
        - `href` is the **name** of the attribute.
        - `https://en.wikipedia.org/wiki/Tim_Berners-Lee` is the **value** of the attribute.
 - Tim Berners-Lee is the **content** of the element.
 - `</a>` is the **closing tag** of the element.

When processed by a **web browser** to be made more user-friendly, the HTML produces the following:

> English scientist [Tim Berners-Lee](https://en.wikipedia.org/wiki/Tim_Berners-Lee) invented the [World Wide Web](https://en.wikipedia.org/wiki/World_Wide_Web) in 1989.

This small amount of HTML — just the anchor element — together with the global reach of URLs as an addressing scheme was enough to make the Web incredibly transformative. The "information superhighway" was born.

---


reference one another. Users can navigate from document to document by following these references, known as **hyperlinks**. The content of the documents is therefore known as **hypertext**.

In order to create documents

Hypertext Markup Language (HTML) is the foundation of the World Wide Web.





 and Hypertext Markup Language (HTML) is the standard markup language for documents designed to be displayed in a web browser. It can be assisted by technologies such as Cascading Style Sheets (CSS) and scripting languages such as JavaScript.
