## Web Mapping 1

https://www.mapbox.com/mapbox-gl-js/api/

Why Mapbox GL

### Setup

At this stage, you should have:
- created a [GitHub](https://github.com) account
- created a new repository 
- cloned a local copy of your repository to your computer using [git](https://git-scm.com/download/mac)
- created the empty files `index.html`, `style.css`, and `map.js` and added them to your repository
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
    font-family: monospace;    
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
console.log("Hello, world!")
```

Unlike HTML and CSS, Javascript is a programming language. HTML and CSS give important information to the browser, but they are like blueprints, whereas Javascript is more like a recipe. We are going to stop short of covering all the programming fundamentals, but you will learn what everything is doing and how to modify code to suit your purposes.

`console.log` is a debug statement--it lets you write to the Javascript console to keep track of what you're doing, which is a very helpful tool. 

If you load your website at this point, you should see ... nothing. 

However, in your browser open `View > Developer > JavaScript Console` (Chrome) or `Develop > Show JavaScript Console` (Safari, with "Show Develop menu in menu bar" turned on in the Preferences under advanced). You should see "Hello World" printed. This means that your `map.js` file has been successfully loaded and you are ready to start programming. If there are other errors, check the format of your HTML and that the script tag which loads the Mapbox javascript are formatted correctly.


### Basic Mapbox GL Javascript

One more setup step: register a [Mapbox](https://www.mapbox.com/signup/) account. Then find your "[Default public token](https://www.mapbox.com/account/)" which you will use in your code.

To begin, replace the debug statement in your js file with the following (minus the comments, which in js are designated by a leading `//`):

```javascript
'use strict'        // let the browser know we're serious

// debug statement letting us know the file is loaded
console.log('Loaded map.js')

// your mapbox token
mapboxgl.accessToken = 'YOUR TOKEN HERE BETWEEN THE QUOTES'
```

You might want to reload your page and check the console to make sure there are no errors--you should just see your debug statement. Now add this to the bottom of your js file:

```javascript
let map = new mapboxgl.Map({
    container: 'map',
    style: 'mapbox://styles/mapbox/satellite-v9',
    center: [-73.96024, 40.80877],
    zoom: 1
})
```

Now we're cooking with gas. If you reload, you should see a satellite image of the world (Web Mercator Projection). Have some fun zooming around.

If you don't see a map, make sure you've followed the syntax exactly, and check for errors in your console. When programming, one misplaced character can break the whole thing (watch your commas and brackets!). The _block_ of code we just added creates a new _variable_ `map` which is a new _instance_ of the `Map` _object_ provided by the Mapbox javascript file we loaded in our HTML. "map" is actually an arbitrary name--you can choose whatever you want. A `Map` object has several attributes that we can change: `container` lets it know the HTML element that will become the map (in this case also called map), `style` defines a data source for the base map, and `center` and `zoom` define the starting coordinates for the map. Change these for `map` and see what happens.

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

Notice how the last style here is attached to a user account. You can customize your own styles with [Mapbox Studio](https://www.mapbox.com/studio/). Once you create a style, you'll just need to find your "Style URL" by clicking "Share &amp; use" and then the "Use" tab and scrolling down. 

There are many other attributes you can add to the Map object, which you can find [here](https://www.mapbox.com/mapbox-gl-js/api/#map).

We're going to add some additional elements to the screen by calling the _method_ `addControl` of `map`. And we're passing `addControl`  instances of more Mapbox objects--a [`NavigationControl`](https://www.mapbox.com/mapbox-gl-js/api/#navigationcontrol) and [`ScaleControl`](https://www.mapbox.com/mapbox-gl-js/api/#scalecontrol), each with their own modifiable parameters.

```javascript
'use strict'

console.log('Loaded map.js')

mapboxgl.accessToken = 'YOUR TOKEN HERE BETWEEN THE QUOTES'

let map = new mapboxgl.Map({
    container: 'map',
    style: 'mapbox://styles/mapbox/satellite-v9',
    center: [-73.96024, 40.80877],
    zoom: 1
})

// create an instance of NavigationControl
let navigation = new mapboxgl.NavigationControl({
    showCompass: false
})

// add the navigation to your map
map.addControl(navigation, 'top-left')

// create an instance of ScaleControl
let scale = new mapboxgl.ScaleControl({
    maxWidth: 80,
    unit: 'imperial'
})

// add the scale to your map
map.addControl(scale, 'bottom-right')
```

A particularly interesting control object to add is [`GeolocateControl`](https://www.mapbox.com/mapbox-gl-js/api/#geolocatecontrol). This uses the capabilities of the browser to track its physical location. You can just add this to the bottom of your code.

```javascript
let geolocate = new mapboxgl.GeolocateControl({
    positionOptions: {
        enableHighAccuracy: true
    },
    trackUserLocation: true,
    showUserLocation: true,
    fitBoundsOptions: {
    }
})

map.addControl(geolocate, 'top-left')

// this is an event handler
geolocate.on('geolocate', function(event) {
    console.log(event.coords)
})

```

After adding this object and reloading the page, you should see the geolocation toggle on your map. Click it and geolocate yourself. 

We added something else here. The `GeolocateControl` has an _event handler_, called `on`, which is a special function that is called in response to an _event_. In this case, the name of the event is `geolocate` and it happens when the user has been successfully geolocated. What does it do? Here we've just asked this function to print `event.coords` to the console. `event` here is a variable which is an object that contains information about the event that just happened. Look in the Javascript console in the browser, and you should find an object:

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

We can see that the event contains the geocoordinates of the user. Let's make use of that "info" element we defined in our HTML. Modify that last block of code so that it looks like this:

```javascript
geolocate.on('geolocate', function(event) {

    // create new variables to store the attributes we're interested in from the event
    let lng = event.coords.longitude
    let lat = event.coords.latitude

    // debug
    console.log('geolocated:', lng, lat)

    // format lng lat values and display them on our 'info' element
    document.getElementById('info').innerHTML = lng.toFixed(5) + "," + lat.toFixed(5) 

})
```

After reloading, you should now see your current longitude and latitude without needing to open your console. We've set up our `GeolocateControl` to track your position, so this function will be called periodically to update its information.

_Note that we've put these on our display with longitude before latitude. Is this standard? Why or why not?_

If you haven't already, now would be great time to `git add -u` your changes so far, `git commit -m "implemented geolocation"` them, and `git push` them to GitHub. Now visit your github.io page using your mobile device to see your geolocating map shine. 


### Markers

You can add a marker to your map like this:

```javascript
let marker = new mapboxgl.Marker()
marker.setLngLat([-73.95997,40.80865])
marker.addTo(map)
```

You can give it a popup like this:

```javascript
let popup = new mapboxgl.Popup()
popup.setText('This is the Center for Spatial Research')
marker.setPopup(popup)
```

Add it and reload your page, you should see a blue marker at the Center for Spatial Research. Click it to see the label.

If you're going to be adding more markers to your map, you're going to need to know where to put them, ie, the longitude/latitude coordinates. We're already displaying our coordinates in the "info" container when we are geolocated--let's make it so that it displays the coordinates where you click on the map as well.

```javascript
map.on('click', function(event) {

    let lng = event.lngLat.lng
    let lat = event.lngLat.lat

    console.log("clicked:", lng, lat)

    document.getElementById('info').innerHTML = lng.toFixed(5) + "," + lat.toFixed(5) 

})
```

This is another event handler, which looks a lot like the one we added to our `GeolocateControl` object. Notice, however, that the `click` event on a `Map` object has a different way of specifying longitude/latitude than the `geolocate` event on the `GeolocateControl` object. Get used to this kind of thing when dealing with Javascript--`console.log` is your friend.

When you reload your page, the "info" container should show the coordinates wherever you click. _Both_ event handlers are now updating "info".


______________________________________________________________________________________________________________


Tutorial written by Brian House for [Fall 2018](https://github.com/brianhouse/mapping-architecture-urbanism-humanities).


