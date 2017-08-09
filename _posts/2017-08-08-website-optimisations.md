---
layout: post
title: Website Optimisations
---

We have become choosy in visiting websites on the basis of their speed of loading. We commute to another for the same reason if it doesn't workout. 

Hence, optimisations are very well needed for the users to stay on the website for long. These optimisations help the users to see visual content in no more than 1-2 secs which help the overall demographics of the website.

> Optimisations help to exercise the importance of content in least amount of time.

## What is Website Optimisation ?
Website Optimisation is an expression used to formulate multiple set of rules to make the website load faster thereby increasing the page speed score and rank well in search engines.

Website Optimisations have a set of steps that help in making the results better :

### Critical CSS

CSS helps in making the content organize in a way making it visually effective. CSS when added as an external stylesheet makes an HTTP call for the file thereby acting as a parser blocker on the other hand when the all the CSS for the page is added as an internal stylesheet makes the html content, less effective  for the SEO perspective. So, Critical CSS is the key to this delimma. 

Critical CSS is the subset of the overall CSS required for the webpage that helps in styling the viewport content on the go and is added as an internal stylesheet. The leftover styles are added as external stylesheets avoiding them to be the parser blocker at the bottom of the body.

### Render Blocking Javascripts

Javascript is the file that helps user in interacting with the webpage. But adding more and more javascript slows down the rendering of the webpage. There are ways to avoid javascripts acting as a render blocker :

* #### Async or Defer the scripts
Async downloads the file during HTML parsing but will act as a render blocking to execute it when file is downloaded. Defer downloads the file during HTML parsing and will execute the file after the parsing is completed.
```
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js" type="text/javascript" defer="true"></script>
```

* #### Injecting the scripts
Injecting the script on load of another script dynamically. When the scripts are injected dynamically then there order of execution is not the same as the order of injection. If the order of execution is important for the well functioning of webpage then inject the script on load of another script.
```
<script>
  var headElement = document.getElementsByTagName('body')[0];
  var count = 0;
  var scripts = ['script-1.js', 'script-2.js', 'script-3.js'];
  function onLoad() {
    count += 1;
    if (count !== scripts.length) {
      addScript();
    }
    return;
  }
  function addScript() {
    var script = document.createElement('script');
    script.src = scripts[count];
    script.onload = onLoad;
    script.async = true;
    headElement.appendChild(script);
  }
</script>
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js" type="text/javascript" defer="true" onload="addScript();"></script>
```

### Reducing the data from the Server

When your application is strictly server side rendered then you really need to decrease the size of the data or html content that helps in constructing the webpage on the browser for the user. 

The more the size of the data, the more time it takes for the browser to render the page thereby effecting user's experience. Hence, lowering the amount of content will make the page render faster.

### Minified form of Javascript and CSS

Add the javascript or css files in minified form which reduces the size of the files to be loaded.
The lower the size of the file, the lesser time it takes to download the file for the webpage. 

The minified file also helps in securing the content of the scripts from the end user as it becomes hard to decode the logic behind the script. Hence, the minfied files should be used in the production environment so that debuging in development mode is easy.

### Image Optimisations

Use smaller images or compressed images that can be easily downloaded on slower internet connection without tampering the webpage rendering.

Some image formats like webp are recommended by browsers like Google and Opera. These formats create smaller, richer images that make the web faster.


### Removing the scripts for mobile Versions

Remove the scripts from the webpage that are not a part of mobile version website. This step reduces the html and makes the rendering faster. 

Hence, there is no need for the scripts to be loaded if they are not used. Try using smaller library version of mobile so that speed of the mobile version is not tampered.

### SEO optimised pages

There are set of rules that make the search engines understand the content of the particular webpages. The set of rules are schema description on the basis of which search engines optimize the search of a relevant content and enhances the ranking of the webpage.

```
<script type="application/ld+json">
{"@context":"http://schema.org","@type":"WebSite","name":"Gradeup","url":"https://gradeup.co/"}
</script>

<script type="application/ld+json">
  {
    "@graph":[
      {
        "@context":"http://schema.org/",
        "@type":"Article",
        "inLanguage":"Language of the article",
        "headline":"The headline of the article",
        "image":{
          "@type":"ImageObject",
          "url":"Main Image Url",
          "width": "dimension of the image",
          "height":"dimension of the image"
        },
        "mainEntityOfPage":"The canonical URL of the article page",
        "datePublished":"Published Date of the url",
        "dateModified":"Modified Date of the url",
        "articleSection":"Origin Area of Content",
        "publisher":{
          "@type":"Organization",
          "name":"Name of the Organization",
          "url":"Website URL of the Organization",
          "logo":{
            "@type":"ImageObject",
            "url":"Logo URL of the Organization",
            "width":"dimension of the image",
            "height":"dimension of the image"
          }
        },
        "description":"Short description of the article",
        "author":{
          "@type":"Person",
          "name":"Author's Name",
          "url":"Author's profile URL if available"
        },
        "url":"The URL of the article page"
      }
    ]
  }
</script>
```

There are other schema designs and SEO optimisation formats that help search engines to rank the pages better and optimize the content searches providing better results.

The AMP alternative should also be adopted as it helps in making the web content experience better for the end users for mobile version. This helps in creating the websites faster, better user experience and high performance on different platforms and devices.

> Here are some links for the optimisations:
  * [SEO Optimisations](https://developers.google.com/search/docs/guides/intro-structured-data)
  * [Page Insights](https://developers.google.com/speed/docs/insights/about)