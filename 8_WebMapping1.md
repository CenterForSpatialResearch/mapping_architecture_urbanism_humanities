## Web Mapping 1

https://www.mapbox.com/mapbox-gl-js/api/

Why Mapbox GL

### Setup

At this stage, you should have:
- created a [GitHub](https://github.com) account
- created a new repository 
- cloned a local copy of your repository to your computer using [git](https://git-scm.com/download/mac)
- created the files `index.html`, `style.css`, and `map.js` and added them to your repository
- used GitHub pages to view your site online

To review: HTML is the structure of a website, CSS is the style, and JavaScript is the functionality or the interaction. Each of these are contained in text files with the appropriate extension--and they each have entirely different syntax. When you are creating a website you are creating a series of linked files that your browser downloads and uses to construct the display. These files can also come from remote resources, such as in the case of javascript libraries or map tiles. 

HTML is defined by a series of tags, which are either in the form `<tag />` or `<tag></tag>`. A comment in HTML, which is simply a non-functional bit of text meant to explicate the code, is in the form `<!-- comment -->`. Reproduce the following in your `index.html` file using your text editor (but you can leave out the comments).

```html
<!DOCTYPE html> <!-- let the browser know this is an HTML file -->

<html>      <!-- everything else is enclosed in html tags -->
<head>      <!-- the head is for metadata prior to building the page -->

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
    <link href='https://api.tiles.mapbox.com/mapbox-gl-js/v0.49.0/mapbox-gl.css' rel='stylesheet' />     
    <link href='style.css' rel='stylesheet' />

    <!-- javascript links -->
    <script src='https://api.tiles.mapbox.com/mapbox-gl-js/v0.49.0/mapbox-gl.js'></script>

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

Unlike HTML and CSS, Javascript is a programming language. HTML and CSS give important information to the browser, but they are  like blueprints, whereas Javascript is more like a recipe. `console.log` is a debug statement--it lets you write to the Javascript console to keep track of what you're doing, which is a very helpful tool. 

If you load your website at this point, you should see ... nothing. 

However, in your browser open `View > Developer > JavaScript Console` (Chrome) or `Develop > Show JavaScript Console` (Safari, with "Show Develop menu in menu bar" turned on in the Preferences under advanced). You should see "Hello World" printed. This means that your `map.js` file has been successfully loaded and you are ready to start programming. If there are other errors, check the format of your HTML and that the script tag which loads the Mapbox javascript are formatted correctly.


### Basic Mapbox GL Javascript

Just kidding, we're not quite ready yet. First, register for a [Mapbox](https://www.mapbox.com/signup/) account. Then find your "[Default public token](https://www.mapbox.com/account/)".

Replace the debug statement in your js file with this (minus the comments, which in js are designated by a leading `//`):

```javascript
'use strict'        // let the browser know we're serious

// debug statement letting us know the file is loaded
console.log('Loaded map.js')

// your mapbox token
mapboxgl.accessToken = 'YOUR TOKEN HERE BETWEEN THE QUOTES'
```

You might want to reload your page and check the console to make sure there are no errors--you should just see your debug statement. Now add this:

```javascript
let my_map = new mapboxgl.Map({
    container: 'map',
    style: 'mapbox://styles/mapbox/satellite-v9',
    center: [-73.96024, 40.80877],
    zoom: 1
})
```

Finally. Now we're cooking with gas. If you reload, you should see a satellite image of the world (Web Mercator Projection). Have some fun zooming around.

If you don't see a map, make sure you've followed the syntax exactly, and check for errors in your console. When programming, one misplaced character can break the whole thing (watch your commas!). The block of code we just added creates a new _variable_ `my_map` which is a new _instance_ of the `Map` _object_ provided by the Mapbox javascript file we loaded in our HTML. A `Map` object has several attributes that we can change: `container` lets it know the HTML element that will become the map, `style` defines a source of map data, and `center` and `zoom` define the starting coordinates for the map. Change these for `my_map` and see what happens.

Other options for style:

```javascript
style: 'mapbox://styles/mapbox/streets-v10',
style: 'mapbox://styles/mapbox/outdoors-v10',
style: 'mapbox://styles/mapbox/light-v9',
style: 'mapbox://styles/mapbox/dark-v9',
style: 'mapbox://styles/mapbox/satellite-v9',
style: 'mapbox://styles/mapbox/satellite-streets-v10',    
style: 'mapbox://styles/mapbox/navigation-preview-day-v2',
style: 'mapbox://styles/mapbox/navigation-preview-night-v2',
style: 'mapbox://styles/mapbox/navigation-guidance-day-v2',
style: 'mapbox://styles/mapbox/navigation-guidance-night-v2',
style: 'mapbox://styles/brianhouse/cjn0u552b52kr2spdz6yhpqj4'
```

Notice how the last style here is attached to a user account. You can customize your own styles with [Mapbox Studio](https://www.mapbox.com/studio/). Once you create a style, you'll just need to find your access token by clicking "Share &amp; use" and then the "Use" tab and scrolling down.

There are many other attributes you can add to the Map object, which you can find [here](https://www.mapbox.com/mapbox-gl-js/api/#map).

We're going to add some additional elements to the screen by calling the _method_ `addControl` of `my_map`. And we're passing `addControl` more Mapbox objects--a [`NavigationControl`](https://www.mapbox.com/mapbox-gl-js/api/#navigationcontrol) and [`ScaleControl`](https://www.mapbox.com/mapbox-gl-js/api/#scalecontrol), each with their own modifiable parameters. Your js file should now look like this:

```javascript
'use strict'

console.log('Loaded map.js')

mapboxgl.accessToken = 'YOUR TOKEN HERE BETWEEN THE QUOTES'

let my_map = new mapboxgl.Map({
    container: 'map',
    style: 'mapbox://styles/mapbox/satellite-v9',
    center: [-73.96024, 40.80877],
    zoom: 1
})

my_map.addControl(new mapboxgl.NavigationControl({
    showCompass: false
}), "top-left")

my_map.addControl(new mapboxgl.ScaleControl({
    maxWidth: 80,
    unit: 'imperial'
}), "bottom-right")
```

A particularly interesting control object to add is [`GeolocateControl`](https://www.mapbox.com/mapbox-gl-js/api/#geolocatecontrol). This uses the capabilities of the browser to track its physical location.

```javascript
my_map.addControl(new mapboxgl.GeolocateControl({
    positionOptions: {
        enableHighAccuracy: true
    },
    trackUserLocation: true,
    showUserLocation: true,
    fitBoundsOptions: {
    }
}).on('geolocate', function (event) {
    console.log(event.coords)
}), "top-left")
```

After adding this object and reloading the page, you should see the geolocation toggle on your map. Click it and geolocate yourself. 

Unlike the previous control objects, `GeolocateControl` has a _handler_, called `on`, which is a special function that is called in response to an _event_. In this case, the name of the event is `geolocate`. So when the user has been geolocated, this handler function is called. What does it do? Here we've just asked it to print `event.coords` to the console. `event` here is a variable which is an object that contains information about the event that just happened. Look in the Javascript console in the browser, and you should find an object:

```javascript
Coordinates {latitude: 40.8087666, longitude: -73.9603058, altitude: null, accuracy: 35, altitudeAccuracy: null, …}
accuracy: 35
altitude: null
altitudeAccuracy: null
heading: null
latitude: 40.8087666
longitude: -73.9603058
speed: null
__proto__: Coordinates
```

