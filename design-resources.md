# Design Resources

As you go about designing and coding your application's screens, here are a few things that you may find useful.

## Readings

I've placed them in rough order of my opinion of their value/length ratio.

 - [Butterick's Typography In Ten Minutes](http://practicaltypography.com/typography-in-ten-minutes.html){:target="_blank"}
 - [7 Rules for Creating Gorgeous UI](https://medium.com/@erikdkennedy/7-rules-for-creating-gorgeous-ui-part-1-559d4e805cda){:target="_blank"} - and [part 2](https://medium.com/@erikdkennedy/7-rules-for-creating-gorgeous-ui-part-2-430de537ba96){:target="_blank"}
 - [Web Design in 4 minutes](http://jgthms.com/web-design-in-4-minutes/){:target="_blank"}
 
## Resources

### Bootstrap

 - [Official Bootstrap Documentation](http://getbootstrap.com/components/){:target="_blank"}
 - [Bootstrap 4 Cheat Sheet](https://hackerthemes.com/bootstrap-cheatsheet/){:target="_blank"} — Since the official docs are really spread out, this cheatsheet can be easier to look through quickly.
 - [Bootswatch](http://bootswatch.com){:target="_blank"} - Cheating! Great for quick styles while prototyping. The previews are also great for looking through quickly for useful components, almost like the Cheat Sheet above.
 - [Shoelace.io](http://shoelace.io){:target="_blank"} - A tool to help you create your Bootstrap grid layout.
 - [Premium Bootstrap themes](https://themes.getbootstrap.com/){:target="_blank"}
 - [Bootstrap.build](https://bootstrap.build/app){:target="_blank"} Generate your own Bootswatch, essentially. If you don't have your own brand identity created yet, you can practice by making a UChicago branded theme; see pages 38-41 of the [Identity Guidelines](https://news.uchicago.edu/sites/default/files/attachments/_uchicago.identity.guidelines.pdf){:target="_blank"}.

### Other design systems

Bootstrap isn't the only design framework in town. Here are a few more, just to give you an idea:

 - [Tailwind CSS](https://tailwindcss.com/components/cards){:target="_blank"} — Imagine if Bootstrap only had utility classes, like the ones for [spacing](https://getbootstrap.com/docs/4.4/utilities/spacing/) and [shadows](https://getbootstrap.com/docs/4.4/utilities/shadows/), but had a lot more than it does; and no components per se.

    That's Tailwind — you're expected to assemble your own cards, alerts, etc, out of the utility classes. Many people like it because it's "lighter" than Bootstrap — it gives you the tools to build up your own, custom framework; but you're still not starting from scratch.
 - [GOV.UK Design System](https://design-system.service.gov.uk/){:target="_blank"} and [U.S. Web Design System](https://designsystem.digital.gov/){:target="_blank"} — Government design systems, with accessibility and consistency as paramount concerns.
 - [Bulma](https://bulma.io/documentation/components/card/){:target="_blank"} and [Foundation](https://get.foundation/sites/docs/card.html){:target="_blank"} — More direct analogs to Bootstrap.


### Fonts

 - [Font Awesome](https://fontawesome.com/icons?d=gallery&m=free){:target="_blank"} - Free, CSS-customizable, perfectly scalable icons for almost everything.
 - [Google Web Fonts](https://www.google.com/fonts){:target="_blank"} - You'll want this after reading [Butterick's](http://practicaltypography.com/typography-in-ten-minutes.html){:target="_blank"}.
 - Google Web Font pairing inspiration:
    - [Typographic Project](http://femmebot.github.io/google-type/){:target="_blank"}
    - [Beautiful Web Type](http://hellohappy.org/beautiful-web-type/?1){:target="_blank"} 
    - [FontPair](https://fontpair.co/){:target="_blank"}
    - [Ultimate Google Web Font Pairings](https://www.reliablepsd.com/ultimate-google-font-pairings/){:target="_blank"}

### Games to get good at CSS

 - [CSS Diner](https://flukeout.github.io/){:target="_blank"} — Become a pro at CSS selectors. Useful if you plan to do a lot of web scraping.
 - [Flexbox Froggy](https://flexboxfroggy.com/){:target="_blank"} — Get a better understanding of Flexbox, the reason that positioning things on the web is no longer a nightmare.

### Stock images

 - [Unsplash](https://unsplash.com/){:target="_blank"}
 - [Stock Up](http://www.sitebuilderreport.com/stock-up){:target="_blank"} - A search engine for free stock photos.
 - [Subtle Patterns](http://subtlepatterns.com){:target="_blank"} - A library of free, tileable, subtle background images.
  
### HTML & CSS references

 - [The CSS Cascade](https://wattenberger.com/blog/css-cascade){:target="_blank"} — Learn how CSS specificity rules actually determine which one of competing rules gets applied to an element.
 - [HTML5 Element List](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5/HTML5_element_list){:target="_blank"} - An excellent, categorized list of HTML elements.
 - [CSS Properties Reference](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Properties_Reference){:target="_blank"}

### Generators

 - [CSS3 Generator](http://css3gen.com/box-shadow/){:target="_blank"} - Generate CSS box shadows, text shadows, and gradients. 
 - [Ultimate CSS Gradient Generator](https://www.colorzilla.com/gradient-editor/){:target="_blank"}

### Colors

 - [Defining colors in CSS](http://web.simmons.edu/~grovesd/comm244/notes/week3/css-colors){:target="_blank"}
 - [Duo](https://duo.alexpate.uk/){:target="_blank"} — Curated color pairings
 - [Adobe Color](http://color.adobe.com){:target="_blank"} - Generate complementary colors.

## Quick links to assets

[BootstrapCDN](https://www.bootstrapcdn.com/){:target="_blank"} helpfully host `bootstrap.css` (and Bootswatches) on their own server.

Bootstrap's Javascript components (like modals, hamburger menu, carousel, etc) depend on another open-source library called jQuery; you can grab it from this CDN: [jQuery CDN](https://code.jquery.com/){:target="_blank"}.

Ultimately, a quick and easy way to get all of Bootstrap and Font Awesome is to include the following in the `<head>` of your document:

```html
<!-- Expand the number of characters we can use in the document beyond basic ASCII 🎉 -->
<meta charset="utf-8">

<!-- Connect Font Awesome CSS -->
<link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.0.2/css/all.css">

<!-- Connect Bootstrap CSS -->
<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/css/bootstrap.min.css">

<!-- Connect Bootstrap JavaScript and its dependencies -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.js"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.4.1/js/bootstrap.bundle.min.js"></script>

<!-- Make it responsive to small screens -->
<meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
```

You can use a [Bootswatch](http://bootswatch.com){:target="_blank"} or [your own customized `bootstrap.css` instead](https://bootstrap.build/app){:target="_blank"} as the `href`, if you like.

Have fun!
