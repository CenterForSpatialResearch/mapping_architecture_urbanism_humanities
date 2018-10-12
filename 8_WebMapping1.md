## Tutorial 8: Web Mapping

We are going to be making webmaps using [Mapbox GL JS](https://www.mapbox.com/mapbox-gl-js/api/). This is a recent javascript library that uses [WebGL](https://en.wikipedia.org/wiki/WebGL) to render interactive maps from vector tiles (as opposed to raster tiles) created with [Mapbox Studio](https://www.mapbox.com/mapbox-studio/). Data for these maps comes from [OpenStreetMap](https://www.openstreetmap.org).

This tutorial will introduce you to the basic setup and what it feels like to develop for a web browser. This will be a very different approach than our experience with QGIS, and it is much more prone to error given the number of new concepts and syntaxes involved. Brackets, commas, quotes, and colons... stay with it—it's ok if you dont understand what everything is doing at first.


### Setup Prerequisites

At this stage, you should have:
- created a [GitHub](https://github.com) account
- created a new repository on GitHub, called `webmap_1`
- set up your repository as a [GitHub page](https://pages.github.com/) (a project site from scratch)
- installed [git](https://git-scm.com/downloads)
- opened your terminal (Applications > Utilities > Terminal.app) and learned how to [navigate](https://www.macworld.com/article/2042378/master-the-command-line-navigating-files-and-folders.html)
- set up git, ie:  
`git config --global user.name 'My Name'`  
`git config --global user.email 'email@wherever.com'`  
...and added [colors](https://nathanhoad.net/how-to-colours-in-git)
- cloned a local copy of your repository to your computer using [git](https://thenewstack.io/tutorial-git-for-absolutely-everyone/):  
`git clone https://github.com/yourusername/webmap_1`
- downloaded a text editor like [Sublime Text](https://www.sublimetext.com/)
- created the empty files `index.html`, `style.css`, and `map.js` with your editor and added them to your repository:  
`git add index.html style.css map.js`
- commited and pushed your changes:  
`git commit -m 'initial import'`  
`git push`  
- viewed the result on your github page


### A web page

HTML is the structure of a web page, css is the style, and javascript is the functionality or the interaction. Each of these are contained in text files with the appropriate extension—and they each have entirely different syntax. When you are creating a website you are creating a series of linked files that your browser downloads and uses to construct the display. These files can also come from remote resources, such as in the case of javascript libraries or map tiles. 

![Directory Structure][DIRECTORY]

HTML is defined by a series of tags, which are either in the form `<tag />` or `<tag></tag>`. A comment in HTML, which is simply a non-functional bit of text meant to explicate the code, is in the form `<!-- comment -->`. Reproduce the following in your `index.html` file using your text editor (you can copy and paste).

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

Notice that we are loading several files within this page. These include a css and javascript file from Mapbox, and our own css and javascript files. The reason `map.js` is loaded within the body and not the head is that it must be loaded _after_ the `map` and `info` elements have been created.

Likewise, put this into your `style.css` file:

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

img {
    width: 300px;
}
```

css defines the display properties for each HTML element. At the moment, our page only has two elements, which you see defined here as `#map` and `#info`. We also have defined a default width for images. Note that a comment in css is designated like `/* comment */`.

Next, your `map.js` file should look like this:

```javascript
console.log("Hello, world!")
```

Unlike HTML and css, javascript is a programming language. HTML and css give important information to the browser, but they are like blueprints, whereas javascript is more like a recipe. Javascript will be our focus for the webmapping exercises. We are going to stop short of covering all general programming fundamentals, but you will learn what everything is doing and how to modify code to suit your purposes.

`console.log` is a debug statement—it lets you write to the javascript console to keep track of what you're doing, which is a very helpful tool. 

At this point, open `index.html` in your browser (preferably Chrome). We will look at it with your github web address later, but for now you are just looking at your site "locally." Your browser should show an address that is something like "`file:///Users/bjh18/Desktop/webmap_1/index.html`" On the page itself you should see ... nothing. 

However, in your browser open `View > Developer > JavaScript Console` (Chrome) or `Develop > Show JavaScript Console` (Safari, with "Show Develop menu in menu bar" turned on in the Preferences under advanced). You should see "[Hello, world!](https://en.wikipedia.org/wiki/%22Hello,_World!%22_program)" printed. This means that your `map.js` file has been successfully loaded and you are ready to start programming. If there are other errors, check the format of your HTML and that the script tag which loads the Mapbox javascript are formatted correctly.

![Hello world][HELLOWORLD]

### Basic Mapbox GL JavaScript

One more setup step: register a [Mapbox](https://www.mapbox.com/signup/) account. Then find your "[Default public token](https://www.mapbox.com/account/)" which you will use in your code.

To begin, replace the debug statement in your js file with the following (minus the comments, which in js are designated by a leading `//`):

```javascript
'use strict'        // let the browser know we're serious

// debug statement letting us know the file is loaded
console.log('Loaded map.js')

// your mapbox token
mapboxgl.accessToken = 'YOUR TOKEN HERE BETWEEN THE QUOTES'
```

You might want to reload your page and check the console to make sure there are no errors—you should just see your debug statement. Now add this to the bottom of your js file:

```javascript
let map = new mapboxgl.Map({
    container: 'map',
    style: 'mapbox://styles/mapbox/satellite-v9',
    center: [-73.96024, 40.80877],
    zoom: 1
})
```

Now we're cooking with gas. If you reload, you should see a satellite image of the world (Web Mercator Projection). Have some fun zooming around.

![World map][WORLDMAP]

If you don't see a map, make sure you've followed the syntax exactly, and check for errors in your console. When programming, one misplaced character can break the whole thing (watch your commas and brackets!). The _block_ of code we just added creates a new _variable_ `map` which is a new _instance_ of the `Map` _object_ provided by the Mapbox javascript file we loaded in our HTML. "map" is actually an arbitrary name—you can choose whatever you want. A `Map` object has several attributes that we can change: `container` lets it know the HTML element that will become the map (in this case also called map), `style` defines a data source for the base map, and `center` and `zoom` define the starting coordinates for the map. Change these for `map` and see what happens.

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

There are many other attributes you can add to the Map object, which you can find [here](https://www.mapbox.com/mapbox-gl-js/api/#map). Try adding `pitch` while centered on Columbia at a high `zoom`, using this custom map style:

```javascript
let map = new mapboxgl.Map({
    container: 'map',
    style: 'mapbox://styles/brianhouse/cjn0u552b52kr2spdz6yhpqj4',
    center: [-73.96216,40.80779],
    zoom: 16,
    pitch: 45
})
```

![3D view][3DMAP]

Now we're going to add some additional elements to the screen by calling the _method_ `addControl` of `map`. And we're _passing_ `addControl`  instances of more Mapbox objects—a [`NavigationControl`](https://www.mapbox.com/mapbox-gl-js/api/#navigationcontrol) and [`ScaleControl`](https://www.mapbox.com/mapbox-gl-js/api/#scalecontrol), each with their own modifiable parameters. Your javascript should look something like this:

```javascript
'use strict'

console.log('Loaded map.js')

mapboxgl.accessToken = 'YOUR TOKEN HERE BETWEEN THE QUOTES'

let map = new mapboxgl.Map({
    container: 'map',
    style: 'mapbox://styles/mapbox/satellite-v9',
    center: [-73.96024, 40.80877],
    zoom: 12
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

![Map controls][CONTROLS]

### Geolocation

A particularly interesting control object to add is [`GeolocateControl`](https://www.mapbox.com/mapbox-gl-js/api/#geolocatecontrol). This uses the capabilities of the browser to track its physical location (_Question for discussion: how does this work?_). You can just add this to the bottom of your code.

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

![Geolocation control][GEOLOCATE]

We added something else here. The `GeolocateControl` has an _event handler_, which is a special function `on` that is called in response to an _event_. In this case, the name of the event is `geolocate` and it happens when the user has been successfully geolocated. What does it do? Here we've just asked this function to print `event.coords` to the console. `event` here is a variable which is an object that contains information about the event that just happened. Look in the javascript console in the browser, and you should find an object:

![Coordinates in console][COORDINATES]

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

After reloading, you should now see your current longitude and latitude without needing to open your console. We've set up our `GeolocateControl` to track your position, so this function will be called periodically to update its information. Note that we've put these on our display with longitude before latitude. _Question for discussion: Is this standard? Why or why not?_

![Coordinates in info box][INFO]

If you haven't already, now would be great time to `git add -u` your changes so far, `git commit -m "implemented geolocation"` them, and `git push` them to GitHub. Now visit your github.io page using your mobile device to see your geolocating map shine. 


#### More event handlers

You're probably going to want to be able to figure out other coordinates on the map other than your geolocated position. We're already displaying our coordinates in the "info" container when we are geolocated—let's make it so that it displays the coordinates where you click on the map as well.


```javascript
map.on('click', function(event) {

    let lng = event.lngLat.lng
    let lat = event.lngLat.lat

    console.log("clicked:", lng, lat)

    document.getElementById('info').innerHTML = lng.toFixed(5) + "," + lat.toFixed(5) 

})
```

This is another event handler, which looks a lot like the one we added to our `GeolocateControl` object. Notice, however, that the `click` event on a `Map` object has a different way of specifying longitude/latitude than the `geolocate` event on the `GeolocateControl` object. Get used to this kind of thing when dealing with javascript—`console.log` is your friend.

When you reload your page, the "info" container should show the coordinates wherever you click. _Both_ event handlers are now updating "info".


### Markers

You can add a marker to your map like this:

```javascript
let marker = new mapboxgl.Marker()
marker.setLngLat([-73.96007,40.80871])
marker.addTo(map)
```

You can give it a popup like this:

```javascript
let popup = new mapboxgl.Popup()
popup.setHTML('This is the Center for Spatial Research')
marker.setPopup(popup)
```

Add it and reload your page, you should see a blue marker at the Center for Spatial Research. Click it to see the label.

![C4SR marker with popup][MARKER]

However, text is boring. We can actually put any HTML elements inside a popup. This is a bit of a recursive wormhole, but our javascript is editing the structure of our page as it runs, and there's a lot of possibilities here. For now, let's just put an HTML image element inside our popup. Find the full URL of an image online. An easy way to do this is to control-click any image you see in your browser and choose "Open Image in New Tab". Then add it to your popup like this:

```javascript
let popup = new mapboxgl.Popup()
popup.setHTML('This is the Center for Spatial Research<br /><img src="https://currystonefoundation.org/wp-content/uploads/2018/05/csf_pr_csr_image5.jpg" />')
marker.setPopup(popup)
```

![C4SR marker with popup and image][IMAGE]

What if you want to add multiple markers to your map? First, we'll need to create an _array_ of the necessary data:

```javascript
let data = [
    {
        location: [-73.96191,40.80762], 
        content: 'I like to eat my lunch here'
    },
    {
        location: [-73.95936,40.80610], 
        content: '15 years ago, you could see over the trees'
    },
    {
        location: [-73.96204,40.80994], 
        content: 'This was once tennis courts'
    },
    ]
```

Be careful with your brackets and braces (and commas). This array contains three objects, each with a location parameter and a content parameter. We want to create a new marker and popup for each of these objects. We can do this in javascript like this:

```javascript
data.forEach(function(d) {

    let marker = new mapboxgl.Marker()    
    marker.setLngLat(d.location)
    marker.addTo(map)  

    let popup = new mapboxgl.Popup()
    popup.setHTML(d.content)
    marker.setPopup(popup)

})
```

This is similar to the marker and popup code we used before, but now we've enclosed it in a block that will be called for _each_ object in our array. Reloading your map should now show the markers on the map.

![Multiple markers on a map][MULTI]

### Deliverables

Make a customized webmap with a set of at least five markers that tells a story about a place in the world. Submit the URL to your github.io site.


______________________________________________________________________________________________________________


Tutorial written by Brian House for Mapping for Architecture, Urbanism, and the Humanities ([Fall 2018](https://github.com/brianhouse/mapping-architecture-urbanism-humanities)).




[DIRECTORY]: Images/webmap_1_01.png
[HELLOWORLD]: Images/webmap_1_02.png
[WORLDMAP]: Images/webmap_1_03.png
[3DMAP]: Images/webmap_1_04.png
[CONTROLS]: Images/webmap_1_05.png
[GEOLOCATE]: Images/webmap_1_06.png
[COORDINATES]: Images/webmap_1_07.png
[INFO]: Images/webmap_1_08.png
[MARKER]: Images/webmap_1_09.png
[IMAGE]: Images/webmap_1_10.png
[MULTI]: Images/webmap_1_11.png


