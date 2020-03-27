# Tutorial 9: Web Mapping 2

In the [previous tutorial](8_WebMapping1.md), we created markers using Mapbox GL JS by declaring them explicitly in our code. Our basemap data came from Mapbox's defaults, which in turn come from [OpenStreetMap](https://www.openstreetmap.org).

In this tutorial, we will create a tileset using external data. Just like QGIS, Mapbox can add layers from vector, raster, and point-based information. Unlike QGIS, which works with maps at a single level of detail, Mapbox converts data to tiles which are delivered to the browser as they are needed. The data in each tile is limited to a bounded geographic area and zoom level, the result of which is that certain things can be visualized only at relevant times, rather than everything needing to be visualized all at once. This allows for the kind of interactivity we have come to expect from web maps, and it also allows us to make local calculations about the user's position in relation to data.

The theme of this tutorial will be a classic element of NYC life.

## Data munging

To begin, we will go through the familiar process of downloading a dataset and preparing it for use. Navigate to the [NYC OpenData](https://opendata.cityofnewyork.us/) portal, and browse the data catalog for 311 Service Requests.

___
![311 Service Requests]
___

View the data, and filter it by `Descriptor: Rat Sightings` and `Created Date is after 09/01/2018`. This will give us rat sightings in NYC since the beginning of the semester, around 2,631 of them (and counting...). Export the data as a CSV file, and call it `rat_sightings.csv`.

___
![Rat filter]
___

Mapbox's native data format is called [GeoJSON](http://geojson.org/). GeoJSON is a data format, but it is also javascript code—GeoJSON defines an array of javascript objects that have a set of expected properties that can be interpreted spatially. While CSV files, shapefiles, and other formats can work with Mapbox, they are converted to GeoJSON behind the scenes.

However, CSVs can have formatting ambiguities that Mapbox does not like, and these can cause errors when uploading data. So we're going to convert our data to GeoJSON before uploading to Mapbox in a more forgiving parser. There are many tools for this, but the simplest for our purposes now is to use an [online converter](http://www.convertcsv.com/csv-to-geojson.htm).

Upload your `rat_sightings.csv` file to the converter and let it do its thing. Download the result as `rat_sightings.geojson`.

___
![Converting to GeoJSON]
___


## Mapbox tilesets

Navigate to [Mapbox Studio](https://www.mapbox.com/studio/), log in with your account, and find the `Tilesets` tab near the top right. Here you will see the built in tilesets provided by Mapbox, as well as option to create a new one.

___
![Mapbox tilesets]
___

Click `New tileset`, and then `Select a file`. Choose your `rat_sightings.geojson` file and upload it. It may take a few minutes to upload and process, but you should end up with a new tileset:

___
![Mapbox upload]
___


## Mapbox styles

On its own, a vector tile does not contain any information about how it should be displayed. This is different than a raster tile, which is an image. Just like HTML needs CSS to tell the browser how to make the elements appear, vector tiles need style information to accompany them.

We're going to create our own Mapbox style. Click on the `Styles` tab in the top right of the page. Choose the option to `Pick a template or upload a style`, and create a map based on the `Dark` template.

___
![Mapbox style chooser]
___

The first thing we'll want to do is manually pan the map so that it centers on New York City. Once we've done that, click on `Map position` in the hovering toolbar on the right side. Lock the default position to the current view, so we don't need to scroll every time.

___
![Position lock]
___

On the left side of the screen is list of all the layers currently used by this map style. These are, first, tilesets. But they are also embedded style information. We'll see how this works by creating our own layer.

In the upper left corner, there is a button `Add layer`. When you click it, you'll see a list of tilesets. `Mapbox streets v7` and `Mapbox terrain v2` are active, because these are used by the `Dark` template. If you click one, it will expand to show multiple types of data, including polygons, lines, and points, any of which can be incorporated into layers on a map. However, we're going to scroll down and look at the unused sources, where you should see the `rat_sightings` (with a random extension) tileset created earlier. If you click it, you'll see just one kind of data, which is points, and if you click _that_, it will add the data to the map.

___
![Choosing rat_sightings]
___

At the top of this pane, there is the title of the layer. Rename name this to remove the random extension, so it's just `rat-sightings`—this will make things easier later on.

On the left side of this panel, there are four tabs that allow you to make adjustments the layer. If you click on the `Type` tab, you'll see options to display the data as several different types. It should have defaulted to `circle`. You'll see this displayed on the map, but with random styling. In the upper right corner, next to the layer tab, you'll see that the `Select data` tab is currently selected—choose `Style` instead. Here we can adjust the look of the circle marker. Go ahead and experiment with the different options.

___
![Circle style]
___

We now have all the rat sightings called into 311 indicated on the map. At this point, we'll want to save our map style. First, let's rename it. Click on the name of the style, currently `Dark`, and call it `NYC Rats`. On the other side of the interface, there is a button, `Publish`. Do this, and you'll see a dialogue box come up where you can find your `Style URL`. Go ahead and copy this, we'll use it later in our javascript.

___
![Mapbox rename]
___
![Mapbox publish]
___

We're not finished yet, however. Markers are one way to present these data, but Mapbox provides some more interesting options. We're going to keep this layer and create another.

Over on the left list of layers, there is an option to duplicate the layer. Do this, select the new layer, and rename it `rat-activity`.

___
![Rat activity]
___

You'll be in the style tab, but click over to `Select data`. Now modify the Type (currently `Circle`) by choosing `Heatmap`. Mapbox will warn us that we'll lose our previous customizations to the layer—but that's ok, because this is a duplicate layer. You might want to zoom in at this point to get a good idea of how it might look when being used on a mobile device. You should see heatmap, once again with a random style:

___
![Heatmap]
___

Mapbox automatically computes heatmap values based on the points in our sightings—play with the heatmap parameters to fine-tune it. The algorithm's default is a bit over-zealous—you might want to set the `radius` value to 20px.

___
![Heatmap style]
___


The heatmap should occlude the sightings markers in the previous layer. That will work just fine for our purposes, so go ahead and publish this map again, and we'll be done with Mapbox Studio for now.


## Start a web project

Follow the same steps as in the [previous tutorial](8_WebMapping1.md):

- create a new repository on your GitHub account, called `nyc_rats`, and make sure you initialize it with a README
- go into the repository settings and make the master branch of your repository a Github page
- clone a local copy of this repository to your computer using [git](https://thenewstack.io/tutorial-git-for-absolutely-everyone/) in the terminal:  
`git clone https://github.com/yourusername/nyc_rats`
- create the empty files `index.html`, `style.css`, and `map.js` with your editor (Sublime Text)

Put this in your `index.html` file:

```html
<!DOCTYPE html>
<html>
<head>
    <meta charset='utf-8' />    
    <title>Rat Map</title>     
    <meta http-equiv='Cache-Control' content='no-cache, no-store, must-revalidate' />
    <meta http-equiv='Pragma' content='no-cache' />
    <meta http-equiv='Expires' content='0' />    
    <meta name='viewport' content='initial-scale=1,maximum-scale=1,user-scalable=no' />
    <link href='https://api.tiles.mapbox.com/mapbox-gl-js/v0.50.0/mapbox-gl.css' rel='stylesheet' />     
    <link href='style.css' rel='stylesheet' />
    <script src='https://api.tiles.mapbox.com/mapbox-gl-js/v0.50.0/mapbox-gl.js'></script>
</head>
<body>
    <div id='map'></div>
    <script src='map.js'></script>    
</body>
</html>
```

And put this into your `style.css` file:

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
```

For `map.js`, we'll use much the same template as the previous tutorial. However, make sure to include your new map style:


```javascript
'use strict'

console.log('Loaded map.js')

mapboxgl.accessToken = 'YOUR TOKEN HERE'

let map = new mapboxgl.Map({
    container: 'map',
    style: 'YOUR STYLE HERE',
    center: [-73.96216,40.80779],
    zoom: 16
})

let navigation = new mapboxgl.NavigationControl({
    showCompass: false
})
map.addControl(navigation, 'top-left')

let scale = new mapboxgl.ScaleControl({
    maxWidth: 80,
    unit: 'imperial'
})
map.addControl(scale, 'bottom-right')

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

geolocate.on('geolocate', function(event) {

})
```

Open `index.html` with your browser, and you should see the map style you created in Mapbox Studio. If not, check the javascript console for errors.

Now add them to your repository, commit, and push your changes:  
`git add index.html style.css map.js`  
`git commit -m 'initial import'`  
`git push`  

And view the map online at http://YOURUSERNAME.github.io/nyc_rats

(Note: if you are getting a 404, or your page is otherwise not showing up, make sure your repository is set to be viewable as a Github Pages website. In the `Settings` of your repository, scroll down to the `Github Pages` section, and if the source reads `None`, then select `master branch` and click `Save`. Your site should now be viewable)


## Place-based interactivity

This map functions well as a reference when viewed on the computer. However, when you view it on a mobile device with your geolocated position it opens up additional possibilities. When you're walking in the streets of NYC, you might not only want to see the heatmap, but a locator that points to the nearest location of a rat sighting. By building this, we will learn how to do map calculations on the fly.

Using Mapbox Studio, we created two layers—one that is our heat map, and a hidden layer that's just markers for individual rat sightings. In javascript, our `map` object has a function, `queryRenderedFeatures`, through which we can retrieve information about those individual sightings. For now, we'll put this inside of a `click` handler like the one we used in the previous tutorial. Add this to the bottom of your javascript:

```javascript
map.on('click', function(event) {

    let features = map.queryRenderedFeatures({ layers: ['rat-sightings'] })
    console.log(features)

})
```
Now, whenever you click the map, you should get a list of features on the `rat-sightings` layer printed out in your console. These will be all the that are _currently_ showing on the map. Click on the arrows in your console to expand one, and you should see all the fields from the original NYC dataset:

___
![Console display]
___

To start with, we're going to find the closest rat sighting to wherever the user clicks on the map. First we will need to get the location of the click. Delete the javascript that you just added, and replace it with the following:

```javascript
map.on('click', function(event) {

    let features = map.queryRenderedFeatures({ layers: ['rat-sightings'] })
    console.log(features)

    let current_location = [event.lngLat.lng, event.lngLat.lat]
    console.log(current_location)

})
```

Test this with the console—each time you click, you should see the coordinates logged.

To find the closest feature to the clicked location, we will cycle through all the features, calculate the distance, and keep track of the shortest one. To do this, we are going to make use of the [turf.js](http://turfjs.org/) library. This library provides a set of javascript functions that we can use in our script. To use it, we'll first need to add a line in our HTML to tell it to load the library. In your `index.html` file, add the following code to the bottom of the `<head>` section, just above the closing tag `</head>`:

```html

<script src='https://cdnjs.cloudflare.com/ajax/libs/Turf.js/5.1.5/turf.min.js'></script>

```

Now we can use turf's distance function in `map.js`. Here is the algorithm with comments. Again, delete the last bit of code you added to `map.js` and replace it with the following:

```javascript
map.on('click', function(event) {

    // get the rat-sightings from the layer data
    let features = map.queryRenderedFeatures({ layers: ['rat-sightings'] })
    console.log(features)

    // get the location of the click
    let current_location = [event.lngLat.lng, event.lngLat.lat]
    console.log("Click location:", current_location)

    // if there aren't any features, don't continue
    if (features.length == 0) return

    // create variables to hold the closest feature found so far
    let closest_distance = Infinity
    let closest_feature = null

    // we're going to check each feature
    for (let feature of features) {

        // calculate the distance using turf
        let distance = turf.distance(turf.point(feature.geometry.coordinates), turf.point(current_location))

        // if the distance is less than the closest distance we've seen so far, update the variables
        if (distance < closest_distance) {
            closest_distance = distance
            closest_feature = feature
        }        

    }

    // closest_distance should now be set to the minimum value
    // closest_feature should be set to the feature itself
    console.log("Closest feature:", closest_feature.geometry.coordinates, "(", closest_distance, "m)")

    // additional handler code goes here

})
```

Run it in your browser and check the console:

![Closest feature]
___

"[Bearing](https://en.wikipedia.org/wiki/Bearing_(navigation))" is the navigational term for the angle of the line between two points. To calculate it between the current location (the click) and the feature location, we'll use turf again. Add this to the bottom of the click handler (before the closing brace and parenthesis):

```javascript
    // calculate bearing
    let bearing = turf.bearing(turf.point(current_location), turf.point(closest_feature.geometry.coordinates))
    console.log("Bearing:", bearing)
```

Now we have all the information we need. We've found the closest feature to the current location, and we've calulated the direction we would need to travel to get there. Let's indicate it on the map with an arrow.

First, we'll need to add a new element in our HTML. Between the two elements already present in the `<body>` section, add a third, so that the `<body>` section now reads:

```html
<body>
    <div id='map'></div>
    <div id='pointer'>↑</div>
    <script src='map.js'></script>        
</body>
```

Notice the up arrow text inside the new 'pointer' div. We could also use an image that we've designed, but this is a quick and dirty way to get it done.

Now that we've added an element to the HTML, we'll need to add style information for it in our CSS. Add the following code to the end of your `style.css` file. This tells the browser to put the `div` container in the center of the screen and make the text big and centered.

```css
#pointer {
    position: absolute;
    top: 50%;
    left: 50%;
    width: 150px;
    height: 200px;
    margin-left: -75px;
    margin-top: -100px;
    font-size: 200px;
    font-family: monospace;
    color: white;
    transform-origin: center;
    text-align: center;
}
```

Now we can add the code to the javascript which will make the arrow turn. This should be within the 'click' handler below our bearing calculation. We're also going to add another line of code which will recenter the map:

```javascript
    // calculate bearing
    let bearing = turf.bearing(turf.point(current_location), turf.point(closest_feature.geometry.coordinates))
    console.log("Bearing:", bearing)

    // turn the pointer in that direction
    var pointer = document.getElementById('pointer')
    pointer.style.transform = 'rotate(' + bearing + 'deg)'

    map.flyTo({ center: current_location })
```

_Think about how this `flyTo` method might be used in another application to create an interactive tour._

Try this out in the browser. Wherever you click, the map should center on that point and the arrow turn to point to the nearest rat sighting.

![Flyto]

Clicking to update the arrow position is not super useful. But it provided a way for us to test out the functionality of our code while building it. What we really want, however, is for the map to follow our geolocated position and for the arrow to update accordingly. All that is required to make that adjustment is to change our code from the `click` event handler to the `geolocate` handler.

Currently, the beginning of our handler looks like this:

```javascript
geolocate.on('geolocate', function(event) {

})

map.on('click', function(event) {

    // get the rat-sightings from the layer data
    let features = map.queryRenderedFeatures({ layers: ['rat-sightings'] })

    ...
```

But we can just delete a few lines to change the functionality:

```javascript
geolocate.on('geolocate', function(event) {

    // get the rat-sightings from the layer data
    let features = map.queryRenderedFeatures({ layers: ['rat-sightings'] })

    ...
```

...whoops, no. As we learned in the previous tutorial, the `geolocate` event gives us coordinates in a different format than the `click` event, so we also have to change what we're putting in our `current_location` variable:
```javascript
    let current_location = [event.coords.longitude, event.coords.latitude]
```

Now the map will center on the user's position and point to the nearest rat sighting, assuming there is one nearby. Try this out on your mobile device.

This is as far as we'll go in this tutorial. But from here, we might want to make the arrow disappear if there are no rat sightings nearby, display the distance to the sighting, add alerts, or other functionality. Additional layers that we create through Mapbox studio might overlay other types of information to help us contextualize what we're seeing, and these layers could be toggled on and off by the user. Think about what might be needed, both in terms of rat sightings and for ways in which you might repurpose this model, and we will discuss in class.


## Deliverables

Submit the github URL to your working rat map.

Extra credit: repurpose this code with a different dataset.


### About those rats...

Curious about rat communication? I was too. Check out the "New York" section of this _New York Times Magazine_ feature from awhile back: [http://nytimes.com/voyages](http://nytimes.com/voyages)


______________________________________________________________________________________________________________


Tutorial written by Brian House for Mapping for Architecture, Urbanism, and the Humanities ([Fall 2018](https://github.com/brianhouse/mapping-architecture-urbanism-humanities)). Edited by Emily Fuhrman for Spring 2020.




[311 Service Requests]: Images/webmap2_311_requests.png
[Rat filter]: Images/webmap2_rat_filter.png
[Converting to GeoJSON]: Images/webmap2_converting_geojson.png
[Mapbox tilesets]: Images/webmap2_mapbox_tilesets.png
[Mapbox upload]: Images/webmap2_mapbox_upload.png
[Mapbox style chooser]: Images/webmap2_style_chooser.png
[Position lock]: Images/webmap2_position_lock.png
[Choosing rat_sightings]: Images/webmap2_choosing_layer.png
[Circle style]: Images/webmap2_circle_style.png
[Mapbox rename]: Images/webmap2_rename.png
[Mapbox publish]: Images/webmap2_mapbox_publish.png
[Rat activity]: Images/webmap2_rat_activity.png
[Heatmap]: Images/webmap2_heatmap.png
[Heatmap style]: Images/webmap2_heatmap_style.png
[Console display]: Images/webmap2_console_display.png
[Closest feature]: Images/webmap2_closest_feature.png
[Flyto]: Images/webmap2_flyto.png
