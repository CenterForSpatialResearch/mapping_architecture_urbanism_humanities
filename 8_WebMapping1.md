## Web Mapping 1

### Setup

At this stage, you should have:
- created a GitHub account
- created a new repository 
- cloned a local copy of your repository to your computer
- created the files `index.html`, `style.css`, and `map.js` and added them to your repository
- used GitHub pages to view your site online

To review: HTML is the structure of a website, CSS is the style, and JavaScript is the functionality or the interaction. Each of these are contained in text files with the appropriate extension--and they each have entirely different syntax. When you are creating a website you are creating a series of linked files that your browser downloads and uses to construct the display. These files can also come from remote resources, such as in the case of javascript libraries or map tiles. 

HTML is defined by a series of tags, which are either in the form `<tag />` or `<tag></tag>`. A comment in HTML, which is simply a non-functional bit of text meant to explicate the code, is in the form `<!-- comment -->`. Reproduce the following in your `index.html` file using your text editor (but you can leave out the comments).

```html
<!DOCTYPE html> <!-- lets the browser know this is an HTML file -->

<html>      <!-- everything is enclosed in html tags -->
<head>      <!-- metadata typically goes in the head -->

    <!-- define the character set -->
    <meta charset='utf-8' />    

    <!-- the title of the page which will show up in the browser bar -->
    <title>Map Test</title>     

    <!-- discourage the browser from caching the page -->    
    <meta http-equiv='Cache-Control' content='no-cache, no-store, must-revalidate' />
    <meta http-equiv='Pragma' content='no-cache' />
    <meta http-equiv='Expires' content='0' />    

    <!-- tells mobile devices how to scale the page -->    
    <meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' />

    <!-- stylesheet links -->
    <link href='https://api.tiles.mapbox.com/mapbox-gl-js/v0.44.1/mapbox-gl.css' rel='stylesheet' />     
    <link href='style.css' rel='stylesheet' />

    <!-- javascript links -->
    <script src='https://api.tiles.mapbox.com/mapbox-gl-js/v0.44.1/mapbox-gl.js'></script>

</head>
<body>      <!-- actual structure begins here -->

    <!-- an element called 'map' -->
    <div id='map'></div>

    <!-- an element called 'info' -->
    <div id='info'></div>

    <!-- load additional javascript -->
    <script src='map.js'></script>    

</body>
</html>
```

Notice that we are loading several files within this page. These include a css and js file from Mapbox, and our own css and js files. The reason `map.js` is loaded within the body and not the head is that it must be loaded _after_ the `map` and `info` elements have been created.

Your `style.css` file should look like this:

```css
body, html {
    margin: 0;
    padding: 0;
}                

#map {
    position: absolute;
    top: 0;
    bottom: 0;
    width: 100%;
}

#info {
    position: absolute;
    top: 0;
    right: 0;
    margin: 10px 10px auto auto;    /* top right bottom left */
    padding: 5px;
    border: 2px solid #ddd;
    border-radius: 5px;
    font-size: 12px;
    text-align: center;
    color: #222;
    background: #fff;
}
```

CSS defines the display properties for each element. At the moment, our page only has two elements, which you see defined here. Note that a comment in CSS is designated like `/* comment */`.

Your `map.js` file should look like this:

```javascript
console.log("Hello World")
```

Unlike HTML and CSS, Javascript is a programming language. HTML and CSS give important information to the browser, but they are  like blueprints, whereas Javascript is more like a recipe. 

If you load your website at this point, you should see ... nothing. 

However, in your browser open `View > Developer > JavaScript Console` (Chrome) or `Develop > Show JavaScript Console` (Safari, with "Show Develop menu in menu bar" turned on in the Preferences under advanced). You should see "Hello World" printed. This means that your `map.js` file has been successfully loaded and you are ready to start programming.


### Basic Mapbox GL

