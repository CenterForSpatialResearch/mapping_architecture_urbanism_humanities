# Tutorial 10: Web Mapping 3

In previous tutorials, we learned how to [set up a Mapbox project](8_WebMapping1.md) and [create custom tilesets based on data](9_WebMapping2.md). We've also placed our own markers and learned how to make calculations based on users' geolocated position.

So far, we've treated datasets as complete—they are datapoints in files of various formats, such as csv, GeoJSON, or shapefiles, that we've added to our maps prior to their use. But as we've seen in class, web-based maps often use databases that are constantly being updated. Furthermore, smart phones are rich sensing devices that allow for the gathering of text, photos, audio, video, GPS position, and accelerometer data. Any of these might be leveraged in myriad ways to contribute to a real-time map.

In this tutorial, we will use the sensing capacity of our devices to make GPS drawings, an activity with a rich precedent including [You Are Here](https://www.macba.cat/en/exhibition-laura-kurgan) by C4SR's Laura Kurgan, [Amsterdam Realtime](https://waag.org/en/project/amsterdam-realtime) by Esther Polak and Jeroen Kee, and drawings by [Jeremy Wood](http://www.gpsdrawing.com/gallery.html). Contemporary devices and Mapbox's API make it straightforward for us to get similar results to what was technically very challenging at the time these works were made.


## Setting up our template

Just like for the [previous web mapping tutorials](9_WebMapping2.md#start-a-web-project), you'll need to set up a new github project. Call this repository "gps_drawing". The `index.html`, `style.css`, and `map.js` can be the same as what we used in the previous tutorial — refer to those tutorials to get your initial files set up. This is a good practice to get familiar with: most programming projects begin in this way, by assembling relevant pieces from past projects. One advantage of using Github is that you'll have all those projects organized and accessible to reference when you need them.

Once you've created the initial files (`index.html`, `style.css`, and `map.js`), we'll need to make a few small changes. In this case, the first customization we'll do is to change the title in the `<title></title>` tags of our `index.html` file: change it to "GPS Drawing".

Additionally, you'll also want to create a new Mapbox style for this project, so that you can customize it without altering your previous maps. Go over to your Mapbox account, click on "Studio" in the upper right corner. Under "Styles," create a new style (refer to the steps in the [previous tutorial](9_WebMapping2.md#mapbox-styles) if you need a reminder how to do this). Once you've created a style, you can rename it by opening it (click "Menu>Details"), and then on the next page, "Edit this style". Once you've renamed the style (you can call it whatever you want, or "GPS_Drawing" for consistency), make sure you copy the new "Style URL", and put that style URL into your code in your `map.js` file. You can edit this style to adjust the aesthetics however you want at a later point if you wish.


### Map programming

While building this map, remember to save periodically and test using your browser and the javascript console. You don't need to push to Github to do that—just open `index.html` directly in your browser. But adding your files, committing them, and pushing to Github is a great way to keep track of your progress and to have a fallback if something breaks.

In `map.js`, you should be creating a map object, adding navigation and scale controls as desired, and including a geolocation control. We'll be begin adding code with the geolocation handler.

```javascript
// create a variable to keep track of the user's current location
// we're going to initialize it to the default center of the map
let current_location = [-73.96216, 40.80779]

// update the variable whenever a geolocation event fires
geolocate.on('geolocate', function(event) {
    current_location = [event.coords.longitude, event.coords.latitude]
    console.log('geolocated', current_location)   
})

// for testing purposes, also update the variable whenever you click on the map
map.on('click', function(event) {
    current_location = [event.lngLat.lng, event.lngLat.lat]
    console.log('clicked', current_location)        
})
```

`current_location` will now hold the most up-to-date coordinates. It will update when geolocation is determined, but since we're programming this on computers, using the `click` handler provides an easier way to test things—you can just click the map to simulate movement.

![Clicked map]

For GPS drawing, we are going to need a way to start and stop "drawing" coordinates. This will make our map begin to feel like a data gathering interface. To do this, we'll add a button. There are three steps to doing this: we'll have to alter the HTML, the css, and the javascript.

First, the HTML. Between the body tags, it should look like this:
```html
<body>
    <div id='map'></div>
    <input type='button' id='draw_btn' value='Start' />
    <script src='map.js'></script>    
</body>
```

We also need to position that `draw_btn` element. We can do that by adding the following block to the css file:
```css
#draw_btn {
    position: absolute;
    left: 10px;
    top: 200px;
}
```

Make sure your files are saved, and reload the page with your browser. You should see the button appear.

![Made button]


Now we can add a reference to it in the javascript, at the very bottom of `map.js` below the map handler:
```javascript
// variable which references the HTML button element
let draw_btn = document.getElementById('draw_btn')

// a handler that is called when the button is clicked
draw_btn.addEventListener('click', function() {

    // print something in the console to test
    console.log('clicked draw_btn')                 

})
```

Test it now. You should see the message appear in the javascript console.

![Clicked button]

To add functionality to this, think about what this button does. It's actually two things: on the first click it should start drawing, and on the second, stop drawing. We're going to create a new function for each of those two behaviors, along with a variable that keeps track of whether we're drawing or not.

<!--BRIAN: there was a shift in use of variables here. i changed all the `drawing` to `active`, to match with your final code-->

```javascript
let active = false       // keeps track of whether or not we're drawing

function startDrawing() {

    active = true

}

function stopDrawing() {

    active = false

}
```

So far this is still just a placeholder. But as we proceed, we'll incrementally add code to these two functions to accomplish more things. First, let's modify `startDrawing` so that it adds a marker at the current location:

```javascript
let active = false
let start_marker = new mapboxgl.Marker()    

function startDrawing() {

    active = true

    start_marker.setLngLat(current_location)
    start_marker.addTo(map)    

}
```

We also want to provide the user with some indication that location is being recorded. We can do this by modifying the button itself.


```javascript
let active = false
let start_marker = new mapboxgl.Marker()    

function startDrawing() {

    active = true

    start_marker.setLngLat(current_location)
    start_marker.addTo(map)

    draw_btn.style['background-color'] = "red"         // make the button red
    draw_btn.style['color'] = "white"                  // make it's text white
    draw_btn.value = 'Stop and save'                   // change the text to the opposite state

}

function stopDrawing() {

    active = false

    draw_btn.style['background-color'] = "white"      // make the button white again
    draw_btn.style['color'] = "black"                 // make the text black
    draw_btn.value = 'Start'                          // change the text

}
```

These functions are still not connected to our button. To do that, we'll need to modify our button's `click` handler. We'll make use of a conditional `if` statement, one of the fundamental logic components of programming. Which code will run is determined by the value of our `active` variable:

```javascript
draw_btn.addEventListener('click', function() {

    console.log('clicked draw_btn')

    if (active) {            // if we're already drawing, stop drawing
        stopDrawing()
    } else {                    // otherwise, start drawing
        startDrawing()
    }

})
```

If you save and test at this point, you should have a button that changes color.

![Button indicator]


So far so good. We have the user interaction down, but now we need to keep track of the path itself as a sequence of points. We'll do that with a variable we will call `path`, which we'll make an array, and we can declare it with our other variables like this:

```javascript
let active = false
let start_marker = new mapboxgl.Marker()     
let path = []               // this array will hold the sequence of points in our path
```

When we hit start, we want to add the current location to the path. Add this line to the `startDrawing` function:

```javascript
function startDrawing() {
    active = true                                
    draw_btn.style['background-color'] = "red"    
    draw_btn.style['color'] = "white"             
    draw_btn.value = 'Stop and save'              

    start_marker.setLngLat(current_location)
    start_marker.addTo(map)

    path.push(current_location)         // add the current location to the path
}
```

Now what? Well, each time `current_location` is updated, we want to add it to the path. So we have to modify our `geolocate` handler and the `click` handler on our map. Once again, we'll use a conditional statement so that it only happens when we are in record mode. Add the following `if` statements to the handlers:

```javascript
geolocate.on('geolocate', function(event) {
    current_location = [event.coords.longitude, event.coords.latitude]
    console.log('geolocated', current_location)   

    if (active) {                           // if we're drawing
        path.push(current_location)         // add the current location to the path
    }

})

map.on('click', function(event) {
    current_location = [event.lngLat.lng, event.lngLat.lat]
    console.log('clicked', current_location)        

    if (active) {                           // if we're drawing
        path.push(current_location)         // add the current location to the path
        console.log(path)                   // ...and for testing purposes, log the path so far to the console.
    }

})
```

We included a `log` statement in the `click` handler so we can see what's going on. Try it by reloading the page. Notice how once you click the record button, every click shows an array in the console. And the array grows with each click. If you open up the array in the console to see the details (click on the small arrow), you can see the coordinates.

![Test coordinates]

Now for the fun part. To show this path on the map, we need to create and continuously modify a map layer via javascript. To do this, we'll use the `addLayer` method of our map object to add a drawing layer. What's tricky about this, however, is that we have to wait until the map style is fully loaded before adding our layer. Therefore, we'll make use of another event handler on our map object, `load`, which is called only once the map is fully in place and ready to be modified.

```javascript
map.on('load', function() {             // 'load' event handler
    map.addLayer({                      // add a layer
        'id': 'drawing',
        'type': 'line',
        'source': {
            'type': 'geojson',
            'data': null
        },
        'layout': {
            'line-cap': 'round',
            'line-join': 'round'
        },
        'paint': {
            'line-color': '#50C3DF',
            'line-width': 5,
            'line-opacity': .8
        }
    })
})
```

You'll notice that the layer we're adding is defined by an elaborate object structure. This includes parameters for `type`, `layout`, and `paint`, all of which you may modify to alter the style of the drawing.

For our purposes now, the most interesting section is `source`. This tells the layer that we will be providing data in the form of geojson. To start with, that data is `null`, meaning there is nothing there. But we can create a geojson object which we will be able to modify on the fly. Add the following variable at the bottom of your code:

```javascript
let geojson = {
    "type": "FeatureCollection",
    "features": []
}
```

This is a minimal definition of a geojson object. Notice that `features` is an array within it. When we start a path, we will also need to create a new feature, and add it to this array. Once again we'll modify our `startDrawing` function:

```javascript
function startDrawing() {
    active = true                              

    start_marker.setLngLat(current_location)
    start_marker.addTo(map)

    draw_btn.style['background-color'] = "red"  
    draw_btn.style['color'] = "white"           
    draw_btn.value = 'Stop and save'            

    path.push(current_location)

    geojson.features.push({                     // add a new feature to the geojson
        "type": "Feature",
        "geometry": {
            "type": "LineString",
            "coordinates": path                 // the coordinates of this feature are our path array
        }
    })    
    map.getSource('drawing').setData(geojson)   // update the drawing layer with the latest geojson

}
```

Notice that last line—each time we update the geojson, we also have to tell the map layer to update. Therefore, we'll also need to add that line to our `geolocate` and `click` handlers:


```javascript
geolocate.on('geolocate', function(event) {
    current_location = [event.coords.longitude, event.coords.latitude]
    console.log('geolocated', current_location)   

    if (active) {
        path.push(current_location)
        map.getSource('drawing').setData(geojson)   // update the layer because the path has changed
    }

})

map.on('click', function(event) {
    current_location = [event.lngLat.lng, event.lngLat.lat]
    console.log('clicked', current_location)        

    if (active) {                
        path.push(current_location)
        console.log(path)           
        map.getSource('drawing').setData(geojson)   // update the layer because the path has changed        
    }

})
```

Now would be a good time to save, reload, and try out the map. If all has gone well, you should be able to click the record button to create a path with subsequent clicks. And if you push it to github and load it on your mobile device, you can walk around to create your path by geolocation.

![Drawing]

And that's it! Play around with your map and create some paths. 

The intent here is that we'll use this app on our mobile devices, not via the web. So we can delete our `click` handler at this point. Better yet, we can "comment it out" so that it will be disabled, but we can still restore it if we need to experiment more. To do this, just add the comment slashes `//` before each line:

```javascript
// map.on('click', function(event) {
//     current_location = [event.lngLat.lng, event.lngLat.lat]
//     console.log('clicked', current_location)        
//
//     if (active) {
//         path.push(current_location)
//         console.log(path)   
//         map.getSource('drawing').setData(geojson)    
//     }
//
// })
```

Now you'll see that clicking on the map has no effect. But geolocating tracking still does.

## Deliverables

Use the interface on your mobile device to draw a picture for the class, and submit a screenshot of what you draw. Yes, you could do this by clicking. But it will be so much more interesting to see it produced on the street, _or_ within your own apartment if you are practicing social distancing.

One caveat to keep in mind—we've built this in javascript, so it's not quite the same as an app you would download from the app store, for example. One big  difference is that it does not run in the background. So as you move around, you'll need to have the browser on your device open for it to be recording your path.

Happy drawing!

______________________________________________________________________________________________________________


Tutorial written by Brian House for Mapping for Architecture, Urbanism, and the Humanities ([Fall 2018](https://github.com/brianhouse/mapping-architecture-urbanism-humanities)). Edited by Emily Fuhrman for Spring 2020.




[mLab account]: Images/10_00_mlabLanding.png
[mLab tiers]: Images/10_01_mlabTiers.png
[mLab create]: Images/10_03_createNew.png
[mLab success]: Images/10_04_mlabSuccess.png
[mLab user setup]: Images/10_04_mlabSuccess_userSetup.png
[mLab user credentials]: Images/10_04_mlabSuccess_userCredentials.png
[mLab new key]: Images/10_05_addAPIkey.png
[mLab copy key]: Images/10_06_copyKey.png
[Clicked map]: Images/10_07_clickedCoords.png
[Made button]: Images/10_08_startButton.png
[Clicked button]: Images/10_09_clickedButton.png
[Button indicator]: Images/10_10_buttonColorChange.png
[Test coordinates]: Images/10_11_storingCoords.png
[Drawing]: Images/10_12_drawingPath.png
[Stop and save]: Images/webmap_3_save.png
[mLab collections]: Images/webmap_3_mlab_collections.png
[mLab data]: Images/webmap_3_mlab_data.png
[Mapbox Duplicate]: Images/webmap_3_duplicate.png
