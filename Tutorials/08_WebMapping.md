## Walkable Distances - Medellín

In this tutorial, you will first make a simple map using the Route 360 API. You will then customize your map to use a Font Awesome icon in place of the default icon. For the Advanced option, you can use the Route360 plugin with the scale. 

An API is an 'Automated Programming Interface', which is how our program can interact with someone else's program and data. There are essentially two ways that APIs work. The first is to allow our program to collect data (output, really) from another program, such as by collecting tweets from Twitter. The other way is to allow our program build on the functionality of another program, such as what we are doing here, using the [Route360 API](https://developers.route360.net/) to use their program (which take a lat/long point we define and calculates the travelable distance in a defined amount of time). 

In our program, we are manually defining these points, but you could make an application that gathers data from the user and creates a map based on the data they provide. The danger of this is that Route360 is proprietary software, meaning that they could discontinue support for this program at any time (FYI, Google Docs is the same type of setup). This is not open source, and you cannot modify the code. However, it's still a really useful tool and an important type of interaction to be aware of. 

Once you have your webmap running, we will define a new icon for the marker using the Font Awesome Library. This is a popular library of icons, and you will likely recognize many of them based on your experience of the internet. 


1. Visit [Route360](https://developers.route360.net/signup/free) and sign up for a free account. 

2. Set up your dependencies - you will need the leaflet css and js files as well as the jquery js file. Add the route360 javascript file to your project after jquery. You will use the web address for the script.

```

	<script src="https://releases.route360.net/r360-js/latest.js"></script>

```

3. Set the style of your map, this is the same for all maps we are making-we want it to take up the entire space it is given
 
```   
     <style>
  #map{position:absolute; top:0; bottom:0; left:0; width: 100%;}
    </style>
```

4. Close the header and open the body, and make a div for the map. Open a script tag.

5. Initialize the map and attribute the technologies and the data

```
var latlon = [LATITUDE, LONGITUDE]

var map = L.map('map').setView(latlon, 13);

// add attribution for the transit data used by the r360 polygon service
map.attributionControl.addAttribution("Lots of data")

```

6. Load the background tile layers from the Route360 tileservice

```

r360.basemap({ style: 'basic', apikey: "A6JVYV2QT7NFKQSRODZZLR3" }).addTo(map);

```


7. Define a new marker


```
var marker = L.marker((latlon)).addTo(map);

```

8. Add the Route360 polygons to the map.


```
var polygonLayer = r360.leafletPolygonLayer().addTo(map);
```

9. Set up all of the options by declaring a 'travelOptions' variable
	1. Get the API key associated with your account
	2. [Find the correct map url](https://developers.route360.net/availability). Click on the region and 'endpoint details' (it may take a second to load).
	3. Add the marker/icon you defined
	4. Pass a list of three durations (in seconds) that you want polygons for (can be any amount of time
	5. Select the [transit type](https://developers.route360.net/help-faq/): 'walk', 'bike', 'transit' or 'car'.

```
var travelOptions = r360.travelOptions();

// please contact us and request your own key
travelOptions.setServiceKey("YOUR_API_KEY");

// set the service url for your area
travelOptions.setServiceUrl(BASEMAP_URL);

// call the marker/icon we defined earlier
travelOptions.addSource(MARKER);

// we want to have polygons for 3 different times (in seconds)
travelOptions.setTravelTimes([D1, D2, D3]);

// traveling method
travelOptions.setTravelType(TRAVEL_TYPE);

```

10. Get the travel time from the options we just defined and draw polygons from it. Add the polygons to the current layer.

```

r360.PolygonService.getTravelTimePolygons(travelOptions, function(polygons){

    // add the returned polygons to the polygon layer
    // and zoom the map to fit the polygons perfectly
    polygonLayer.clearAndAddLayers(polygons, true);
});
```

11. Close the script, body, and html tags.

12. Check your map to see if it works


### Font Awesome Icons

1. Add your dependencies

The Font Awesome Icons are another library that needs to be brought in to the project. We will add these packages to the header. Start with the stylesheets. Add the bootstrap and awesome markers stylesheets AFTER the stylesheets you have already imported. Bootstrap is necessary for the Awesome Markers library to work. You will use a Bootstrap template to submit your final project as well. It is still best practice to reference it in your embedded map as well (though we could do some dort of inheritance). 


```
  <link href="//maxcdn.bootstrapcdn.com/font-awesome/4.2.0/css/font-awesome.min.css" rel="stylesheet">
  <link rel="stylesheet" href="css/leaflet_awesome_markers.css">
  ```

Add the awesome markers javascript file AFTER the other js files

```  <script src="js/leaflet_awesome_markers.js"></script>

```


2. Find where you had previously defined your marker and comment that line of code out. Define a new variable that will hold the new marker. I'm going to call mine aMarker. Call the Awesome Markers library through the Leaflet library and use the icon function. The prefix is 'fa' for 'font awesome'. We will use the car since our example code is for travel by car. You can pick other icons [here](http://fontawesome.io/icons/). I'm going to specify the color as black. 

```
//define new marker
// var marker = L.marker((latlon)).addTo(map);

var aMarker = L.AwesomeMarkers.icon({
       prefix: 'fa', //font awesome
       icon: 'car', 
       iconColor: 'black',
    })

var marker = L.marker((latlon), {icon: aMarker}).addTo(map);

```

3. Revisit your map


### Next steps

From here, you can export the GeoJSON, create a map with a scale bar, find the average from [two different points](https://developers.route360.net/services/polygon), or change [the basemap style](https://developers.route360.net/services/basemaps), perform calculations, enhance user experience, etc. Guidelines for working with this app are [here](https://developers.route360.net/login).


To complete this tutorial, submit your working FOLDER.

______________________________________________________________________________________________________________

Tutorial written by Michelle McSweeney, for Conflict Urbanism: InfaPolitics, a seminar taught at Columbia University by the [Center for Spatial Research](http://c4sr.columbia.edu) in Fall 2017.

