# Tutorial 10: Web Mapping 3

In previous tutorials, we learned how to [set up a Mapbox project](8_WebMapping1.md) and [create custom tilesets based on data](9_WebMapping2.md). We've also placed our own markers and learned how to make calculations based on users' geolocated position.

So far, we've treated datasets as complete—they are datapoints in files of various formats, such as csv, GeoJSON, or shapefiles, that we've added to our maps prior to their use. But as we've seen in class, web-based maps often use databases that are constantly being updated. Furthermore, smart phones are rich sensing devices that allow for the gathering of text, photos, audio, video, GPS position, and accelerometer data. Any of these might be leveraged in myriad ways to contribute to a real-time map.

In this tutorial, we will use the sensing capacity of our devices to make GPS drawings, an activity with a rich precedent including [You Are Here](https://www.macba.cat/en/exhibition-laura-kurgan) by C4SR's Laura Kurgan, [Amsterdam Realtime](https://waag.org/en/project/amsterdam-realtime) by Esther Polak and Jeroen Kee, and drawings by [Jeremy Wood](http://www.gpsdrawing.com/gallery.html). Contemporary devices and Mapbox's API make it straightforward for us to get similar results to what was technically very challenging at the time these works were made.


## Databases

Our web-based maps run via javascript in the browser. For them to interact with a database, the database also has to be accessible online. For many projects, you'll want to set up your own database server. That's beyond the scope of our exercise now, however, so we are going to make use of a service, [mLab](https://mlab.com/), that will host a database that we can use (yet another account to create!). 

![mLab account]

After creating your account and logging in, create a new Mongo DB Deployment. Mongo DB is a type of database that stores javascript-style objects such as the GeoJSON format we have previously discussed, so it will be ideal for our purposes. mLab manages the deployment of the database, and it gives us several options of where to host it in the "cloud" (aka massive datacenters owned and operated by Amazon, Google, and Microsoft). We'll use Amazon Web Services, with a free sandbox plan, and host it in the `US East` region. Call it `drawing-db`.

![mLab sandbox]

![mLab confirmation]

Click the blue button in the bottom right corner that says "Submit Order", and you should be up and running.

Once you've created and deployed your database, you'll need to get your API key. You'll find this under your "user" settings. Click the blue link that has your username in the upper right corner, and scroll down to the section that says "API Key." Copy your key (paste it into another document or note application if you need to). You'll also need to `Enable Data API Access`.

![mLab db user]
![mLab API Enable]


## Setting up our template

Just like for the [previous web mapping tutorials](9_WebMapping2.md), you'll need to set up a new github project. `index.html`, `style.css`, and `map.js` can be the same as what you've used before to begin with. In fact, most programming projects begin in this way, by assembling relevant pieces from past projects. One advantage of using Github is that you'll have all those projects organized and accessible to reference when you need them. In this case, the first customization we'll do is to change the title in the `<title></title>` tags. You'll also want to create a new Mapbox style for this project that you can customize without altering your previous maps—you'll put that style URL into your code.


## Interfacing with the database

We're going to use some prewritten code to interface with the mLab API. This consists of two functions—inserting data, and recalling data. Save this code in a new file, `db.js`, and put in your API key from mLab. 

```javascript
let base = "https://api.mlab.com/api/1/databases/"
let apiKey = YOURKEYHERE
let database = "drawing-db"

class DB {

    get(f) {
        console.log('GET')
        let url = base + database + "/collections/paths?apiKey=" + apiKey
        let request = new XMLHttpRequest()
        request.open('GET', url)
        request.onload = function() {
          if (this.status >= 200 && this.status < 400) {
            f(JSON.parse(this.response))
          } else {
            console.log(this.status)
            console.log(this.response)
          }
        }
        request.onerror = function() {
            console.log(this.status)
            console.log(this.response)
        }
        request.send()
    }

    insert(path) {
        console.log("INSERT")
        let url = base + database + "/collections/paths?apiKey=" + apiKey
        let request = new XMLHttpRequest()
        request.open('POST', url)
        request.setRequestHeader('Content-Type', 'application/json')
        request.onload = function() {
          if (this.status >= 200 && this.status < 400) {
            console.log(this.response)
          } else {
            console.log(this.status)
            console.log(this.response)
          }
        }
        request.onerror = function() {
            console.log(this.status)
            console.log(this.response)
        }
        let encoded = JSON.stringify({path: path})
        console.log(encoded)
        request.send(encoded)
    }

}
```

To use this in your map code, you'll also need to load `db.js` within your HTML. You can do this by adding a script tag within the `<head>` portion of your HTML.

```html
<head>

...
    <script src='https://api.tiles.mapbox.com/mapbox-gl-js/v0.50.0/mapbox-gl.js'></script>
    <script src='db.js'></script>    
</head>
<body>

...

```


### Map programming

While building this map, remember to save periodically and test using your browser and the javascript console. You don't need to push to Github to do that—just open `index.html` directly in Chrome or Safari. But adding your files, committing them, and pushing to Github is a great way to keep track of your progress and to have a fallback if something breaks.

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

```javascript
let drawing = false       // keeps track of whether or not we're drawing

function startDrawing() {

    drawing = true

}

function stopDrawing() {

    drawing = false

}
```

So far this is still just a placeholder. But as we proceed, we'll incremently add code to these two functions to accomplish more things. First, let's modify `startDrawing` so that it adds a marker at the current location:

```javascript
let drawing = false
let start_marker = new mapboxgl.Marker()    

function startDrawing() {

    drawing = true

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

    record_btn.style['background-color'] = "red"         // make the button red
    record_btn.style['color'] = "white"                  // make it's text white
    record_btn.value = 'Stop and save'                   // change the text to the opposite state

}

function stopDrawing() {

    active = false

    record_btn.style['background-color'] = "white"      // make the button white again
    record_btn.style['color'] = "black"                 // make the text black
    record_btn.value = 'Start'                          // change the text

}
```

These functions are still not connected to our button. To do that, we'll need to modify our button's `click` handler. We'll make use of a conditional `if` statement, one of the fundamental logic components of programming. Which code will run is determined by the value of our `active` variable:

```javascript
record_btn.addEventListener('click', function() {
  
    console.log('clicked record_btn')
  
    if (active) {            // if we're already drawing, stop drawing
        stopDrawing()
    } else {                    // otherwise, start drawing
        startDrawing()
    }

})
```

If you save and test at this point, you should have a button that changes color.

![Button indicator]


So far so good. We have the user interaction down, but now we need to keep track of the path itself as a sequence of points. We'll do that with an array, which we declare with our other variables like this:

```javascript
let active = false
let start_marker = new mapboxgl.Marker()     
let path = []               // this array will hold the sequence of points in our path
```

When we hit start, we want to add the current location to the path.

```javascript
function startDrawing() {
    drawing = true                                
    record_btn.style['background-color'] = "red"    
    record_btn.style['color'] = "white"             
    record_btn.value = 'Stop and save'              

    start_marker.setLngLat(current_location)
    start_marker.addTo(map)

    path.push(current_location)         // add the current location to the path
}
```

Now what? Well, each time `current_location` is updated, we want to add it to the path. So we have to modify our `geolocate` handler and the `click` handler on our map. Once again, we'll use a conditional statement so that it only happens when we are in record mode.

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

For our purposes now, the most interesting section is `source`. This tells the layer that we will be providing data in the form of geojson. To start with, that data is `null`, meaning there is nothing there. But we will create a geojson object which we will be able to modify on the fly.

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
    record_btn.style['background-color'] = "red"  
    record_btn.style['color'] = "white"           
    record_btn.value = 'Stop and save'            

    start_marker.setLngLat(current_location)
    start_marker.addTo(map)

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


### Database functions

One thing that our code doesn't yet do, however, is save paths in the database. If you reload the page, the paths will disappear. We've already included `db.js` in our setup—now we just have to make the appropriate calls.

We want to save a path every time the `stopDrawing` function is called, so we'll add that to the function. We'll also make sure to reset our path variable:

```javascript
function stopDrawing() {

    active = false

    record_btn.style['background-color'] = "white"  
    record_btn.style['color'] = "black"             
    record_btn.value = 'Start'                      

    db.insert(path)                         // insert the path into the database
    path = []                               // reset the path

}
```

Now, when you complete a path by clicking "Stop and save" on your map, it really is saving, and you'll see that in your console:

![Stop and save]


We're now saving the paths that we create. However, they're still not showing up when we reload the page. To do that, we'll need to query the database right away and draw those paths from the get-go. We'll modify the same `load` event handler that we used to create our layer, which will ensure that everything happens in the correct order.

```javascript
map.on('load', function() {
    map.addLayer({
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

    // get the previously created paths from the database and add them as features
    db.get(function(data) {
        for (let item of data) {
            if (!item.path) continue
            geojson.features.push({
                "type": "Feature",
                "geometry": {
                    "type": "LineString",
                    "coordinates": item.path
                }
            })
            map.getSource('drawing').setData(geojson)
        }
    })

})
```

Phew. That's it! Play around with your map and create some paths. When you reload the page, they should show up again. You're live!

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

One last thing: we didn't add any functionality to _delete_ paths, but you can do this through your mLab account. If you click on your database, you'll see your "Collections". One of those will be "paths", and if you click on that, you can see the coordinate data in GeoJSON format, as well as edit it directly.

![mLab collections]
![mLab data]

## Deliverables

Use the interface on your mobile device to draw a picture for the class, and submit the url to your map. Yes, you could do this by clicking. But it will be so much more interesting to see it produced on the street.

One caveat to keep in mind—we've built this in javascript, so it's not quite the same as an app you would download from the app store, for example. One big  difference is that it does not run in the background. So as you move around, you'll need to have the browser on your device open for it to be recording your path.

Happy drawing!

______________________________________________________________________________________________________________


Tutorial written by Brian House for Mapping for Architecture, Urbanism, and the Humanities ([Fall 2018](https://github.com/brianhouse/mapping-architecture-urbanism-humanities)).




[mLab account]: Images/webmap_3_mlab_account.png
[mLab sandbox]: Images/webmap_3_mlab_sandbox.png
[mLab confirmation]: Images/webmap_3_mlab_confirmation.png
[mLab db user]: Images/webmap_3_mlab_db_user.png
[mLab API Key]: Images/webmap_3_mlab_api_key.png
[mLab API Enable]: Images/webmap_3_mlab_api_enable.png
[Clicked map]: Images/webmap_3_clicked_map.png
[Made button]: Images/webmap_3_made_button.png
[Clicked button]: Images/webmap_3_clicked_button.png
[Button indicator]: Images/webmap_3_button_indicator.png
[Test coordinates]: Images/webmap_3_test_coordinates.png
[Drawing]: Images/webmap_3_drawing.png
[Stop and save]: Images/webmap_3_save.png
[mLab collections]: Images/webmap_3_mlab_collections.png
[mLab data]: Images/webmap_3_mlab_data.png


