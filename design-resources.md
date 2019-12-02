# Design Resources

As you go about designing and coding your application's screens, here are a few things that you may find useful.

## Readings

I've placed them in rough order of my opinion of their value/length ratio.

 - [Butterick's Typography In Ten Minutes](http://practicaltypography.com/typography-in-ten-minutes.html)
 - [Web Design in 4 minutes](http://jgthms.com/web-design-in-4-minutes/)
 - [7 Rules for Creating Gorgeous UI](https://medium.com/@erikdkennedy/7-rules-for-creating-gorgeous-ui-part-1-559d4e805cda) - and [part 2](https://medium.com/@erikdkennedy/7-rules-for-creating-gorgeous-ui-part-2-430de537ba96)
 - [Google's Material design guidelines](https://material.io/guidelines/material-design/introduction.html)
 
## Resources

In rough order of how often I use them.

 - [Official Bootstrap Documentation](http://getbootstrap.com/components/)
 - [Bootswatch](http://bootswatch.com) - Cheating. Great for quick styles while prototyping.
 - [Bootstrap 4 Cheat Sheet](https://hackerthemes.com/bootstrap-cheatsheet/)
 - [Font Awesome](http://fortawesome.github.io/Font-Awesome/icons/) - Free, CSS-customizable, perfectly scalable icons for almost everything.
 - [Google Web Fonts](https://www.google.com/fonts) - You'll want this after reading Butterick's.
 - [Stock Up](http://www.sitebuilderreport.com/stock-up) - A search engine for free stock photos.
 - [Shoelace.io](http://shoelace.io) - A tool to help you create your Bootstrap grid layout.
 - Google Web Font pairing inspiration:
    - [Beautiful Web Type](http://hellohappy.org/beautiful-web-type/?1) 
    - [Typographic Project](http://femmebot.github.io/google-type/)
    - [Ultimate Google Web Font Pairings](https://www.reliablepsd.com/ultimate-google-font-pairings/)
    - [FontPair](https://fontpair.co/)
 - [HTML5 Element List](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/HTML5/HTML5_element_list) - An excellent, categorized list of HTML elements.
 - [Firefox Tilt3D Extension](https://addons.mozilla.org/en-US/firefox/addon/tilt/) - A tool to help you visualize what's going wrong with your layout.
 - [CSS3 Generator](http://css3gen.com/box-shadow/) - Generate CSS box shadows, text shadows, and gradients. 
 - [Subtle Patterns](http://subtlepatterns.com) - A library of free, tileable, subtle background images.
 - [Adobe Color](http://color.adobe.com) - Generate complementary colors. 
 - [Premium Bootstrap themes](https://themes.getbootstrap.com/)
 - [Bootstrap.build](https://bootstrap.build/app) (you can practice by making a UChicago branded theme; see pages 38-41 of our [Identity Guidelines](https://news.uchicago.edu/sites/default/files/attachments/_uchicago.identity.guidelines.pdf))

### Assets

 - These folks helpfully host `bootstrap.css` (and Bootswatches) on their own server: [BootstrapCDN](https://www.bootstrapcdn.com/)
 - Bootstrap's Javascript components (like modals, hamburger menu, carousel, etc) depend on another open-source library called jQuery; you can grab it from this CDN: [jQuery CDN](https://code.jquery.com/).
 - Ultimately, a quick and easy way to get all of Bootstrap and Font Awesome is to include the following in the `<head>` of your document:

    ```html
    <!-- Expand the number of characters we can use in the document beyond basic ASCII 🎉 -->
    <meta charset="utf-8">

    <!-- Connect Font Awesome stylesheet -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/5.11.2/css/all.min.css">

    <!-- Connect Bootstrap stylesheet -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css">

    <!-- Connect Bootstrap JavaScript and its dependencies -->
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.4.1/jquery.min.js"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.bundle.min.js"></script>

    <!-- Make it responsive to small screens -->
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    ```

You can use a Bootswatch instead as the `href`, if you like.
