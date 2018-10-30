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

Once you've created and deployed your database, you'll need to get your API key. You'll find this under your "user" settings. You'll also need to `Enable Data API Access`.

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

// update the variable whenever a geolcation event fires
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

For GPS drawing, we are going to need a way to start and stop "recording" coordinates. This will make our "map" begin to feel like a data gathering interface. To do this, we'll add a button. There are three steps to doing this: we'll have to alter the HTML, the css, and the javascript.

First, the HTML. Between the body tags, it should look like this:
```javascript
    <body>
        <div id='map'></div>
        <input type='button' id='record_btn' value='Start' />
        <script src='map.js'></script>    
    </body>
```

We also need to position that `record_btn` element. We can do that by adding the following block to the css file:
```css
#record_btn {
    position: absolute;
    left: 10px;
    top: 200px;
}
```

Make sure your files are saved, and reload the page with your browser. You should see the button appear.

Now we can add a reference to it in the javascript, at the very bottom below the map handler:
```javascript
    // variable which references the HTML button element
    let record_btn = document.getElementById('record_btn')

    // a handler that is called when the button is clicked
    record_btn.addEventListener('click', function() {
        console.log('clicked record_btn')                   // this will show up in the console so we can test that the button is working
    })
```

Test it now. You should see the messages appear in the javascript console.

To add functionality to this, think about what this button does. It's actually two things: on the first click it should start recording, and on the second, stop. To start with, we're going to create functions for each of those two behaviors.




 feature that starts and stop 


For GPS drawing, we're going to need to keep track of our path as a sequence of points.

```javascript
// this array will hold the sequence of points in our path
let path = []
```






## Deliverables





______________________________________________________________________________________________________________


Tutorial written by Brian House for Mapping for Architecture, Urbanism, and the Humanities ([Fall 2018](https://github.com/brianhouse/mapping-architecture-urbanism-humanities)).




[mLab account]: Images/webmap_2_mlab_account.png
[mLab sandbox]: Images/webmap_2_mlab_sandbox.png
[mLab confirmation]: Images/webmap_2_mlab_confirmation.png
[mLab db user]: Images/webmap_2_mlab_db_user.png
[mLab API Key]: Images/webmap_2_mlab_api_key.png
[mLab API Enable]: Images/webmap_2_mlab_api_enable.png


