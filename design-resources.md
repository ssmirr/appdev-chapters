# Design Resources

As you go about designing and coding your application's screens, here are a few things that you may find useful.

## Readings

I've placed them in rough order of my opinion of their value/length ratio.

 - [Butterick's Typography In Ten Minutes](http://practicaltypography.com/typography-in-ten-minutes.html){:target="_blank"} — the whole thing is worth a read.
 - [7 Rules for Creating Gorgeous UI](https://medium.com/@erikdkennedy/7-rules-for-creating-gorgeous-ui-part-1-559d4e805cda){:target="_blank"} - and [part 2](https://medium.com/@erikdkennedy/7-rules-for-creating-gorgeous-ui-part-2-430de537ba96){:target="_blank"}
 - [Web Design in 4 minutes](http://jgthms.com/web-design-in-4-minutes/){:target="_blank"}
 
## Resources

### Bootstrap

 - [Official Bootstrap Documentation](http://getbootstrap.com/components/){:target="_blank"}
 - [Bootstrap 4 Cheat Sheet](https://hackerthemes.com/bootstrap-cheatsheet/){:target="_blank"} — Since the official docs are really spread out; this cheatsheet is easier to look through quickly.
 - [Bootswatch](http://bootswatch.com){:target="_blank"} - Cheating. Great for quick styles while prototyping. The previews are also great for looking through quickly for useful components, almost like the Cheat Sheet above.
 - [Shoelace.io](http://shoelace.io){:target="_blank"} - A tool to help you create your Bootstrap grid layout.
 - [Premium Bootstrap themes](https://themes.getbootstrap.com/){:target="_blank"}
 - [Bootstrap.build](https://bootstrap.build/app){:target="_blank"} (you can practice by making a UChicago branded theme; see pages 38-41 of our [Identity Guidelines](https://news.uchicago.edu/sites/default/files/attachments/_uchicago.identity.guidelines.pdf){:target="_blank"}

### Fonts

 - [Font Awesome](https://fontawesome.com/icons?d=gallery&m=free){:target="_blank"} - Free, CSS-customizable, perfectly scalable icons for almost everything.
 - [Google Web Fonts](https://www.google.com/fonts){:target="_blank"} - You'll want this after reading [Butterick's](http://practicaltypography.com/typography-in-ten-minutes.html){:target="_blank"}.
 - Google Web Font pairing inspiration:
    - [Beautiful Web Type](http://hellohappy.org/beautiful-web-type/?1){:target="_blank"} 
    - [Typographic Project](http://femmebot.github.io/google-type/){:target="_blank"}
    - [Ultimate Google Web Font Pairings](https://www.reliablepsd.com/ultimate-google-font-pairings/){:target="_blank"}
    - [FontPair](https://fontpair.co/){:target="_blank"}

### Stock images

 - [Unsplash](https://unsplash.com/){:target="_blank"}
 - [Stock Up](http://www.sitebuilderreport.com/stock-up){:target="_blank"} - A search engine for free stock photos.
 - [Subtle Patterns](http://subtlepatterns.com){:target="_blank"} - A library of free, tileable, subtle background images.
  
### HTML & CSS references

 - [HTML5 Element List](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5/HTML5_element_list){:target="_blank"} - An excellent, categorized list of HTML elements.
 - [CSS Properties Reference](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Properties_Reference){:target="_blank"}

### Generators

 - [CSS3 Generator](http://css3gen.com/box-shadow/){:target="_blank"} - Generate CSS box shadows, text shadows, and gradients. 
 - [Ultimate CSS Gradient Generator](https://www.colorzilla.com/gradient-editor/){:target="_blank"}

### Colors

 - [Duo](https://duo.alexpate.uk/){:target="_blank"} — Curated color pairings
 - [Adobe Color](http://color.adobe.com){:target="_blank"} - Generate complementary colors.

## Quick links to assets

 - These folks helpfully host `bootstrap.css` (and Bootswatches) on their own server: [BootstrapCDN](https://www.bootstrapcdn.com/){:target="_blank"}
 - Bootstrap's Javascript components (like modals, hamburger menu, carousel, etc) depend on another open-source library called jQuery; you can grab it from this CDN: [jQuery CDN](https://code.jquery.com/){:target="_blank"}.
 - Ultimately, a quick and easy way to get all of Bootstrap and Font Awesome is to include the following in the `<head>` of your document:

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
