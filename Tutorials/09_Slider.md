## Timeline Slider - Mumbai

To illustrate a sequence of events, you will make a timeline slider. This is a manual slider using [this plugin](https://github.com/dwilhelm89/LeafletSlider) built be Dennis Wilhelm. You will first make a time-based slider. Then, time permitting, you will change the icon to use the font-awesome library. This can be customized based on any column in your geojson file. 

The geojson file for this project comes from Tutorial 3: Annotating with Leaflet.js. For instructions on how to export a geojson from QGIS, please revisit that tutorial. 

First you will get your code running with the time slider only. Then, you will add the popups. Finally, time permitting, you turn the historical map that you georeferenced into a tilelayer using Mapbox and import it into your project. This is the first step in adding a before/after slider.

If you would rather make the animated version, [the plugin for that is here](http://skeate.github.io/Leaflet.timeline/). It is slightly more complicated, so would be a good next step after completing this tutorial. However, if you are comfortable with HTML, CSS, and JS, you may want to go straight to the animated version. 

    
1. Load your dependencies. You will need the leaftlet and jquery stylesheets.

****Get the Leaflet stylesheet from the CDN and remove the one that we downloaded. This plugin requires this version of leaflet to display the popups****

    
```
    <link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.7.2/leaflet.css" />
    <link rel="stylesheet" href="http://code.jquery.com/ui/1.9.2/themes/base/jquery-ui.css" type="text/css">

```

You will need the leaflet, jquery, jquery-ui, jqueryui-touch (for touchscreens), and the SliderControl javascript libraries. We know that we will need all of these because we read the ReadMe file associated with the plugin.

****Again, retrieve these from the CDN as there is a version issue****

```
    <script src="http://cdn.leafletjs.com/leaflet-0.7.2/leaflet.js"></script>
    <script src="http://code.jquery.com/jquery-1.9.1.min.js"></script>
    <script src="http://code.jquery.com/ui/1.9.2/jquery-ui.js"></script>

    <!-- Include this library for mobile touch support  -->
    <script src="http://cdnjs.cloudflare.com/ajax/libs/jqueryui-touch-punch/0.2.2/jquery.ui.touch-punch.min.js"></script>

    <script src="SliderControl.js"></script>
    ```
    
2. Set the style of your map, this is the same for all maps we are making-we want it to take up the entire space it is given
 
```   
     <style>
  #map{position:absolute; top:0; bottom:0; left:0; width: 100%;}
    </style>
```

3. Close the header and open the body, and make a div for the map. Open a script tag.

4. Set up our variables for the slider, the map, and the special markers. We will set the slider to null, the map to the latitude, longitude, and zoom level we want, and the marker to the same simple black marker we used in Tutorial 3. We will use all of these later - we just need them all to exist before we start writing our code. 

```
// make the slider exist
    var sliderControl = null;
    
// create a map variable with the center point and zoom specified
    var map = L.map('map').setView([LATITUDE, LONGITUDE], ZOOM);

//Specify an icon
var myIcon = L.icon({
iconUrl: 'css/images/maps-mark.png',
  iconSize: [20, 20],
  popupAnchor: [0, -16]
});
```

5. Load your tile layer. We will use stamen's terrian map. It's nice, it's basic, you can style it with other tiles should you wish. Be sure to add an attribution for both the tiles and the data. 

```
        L.tileLayer('http://tile.stamen.com/terrain/{z}/{x}/{y}.png', {
        attribution: 'Map tiles by <a href="http://stamen.com">Stamen Design</a>, under <a href="http://creativecommons.org/licenses/by/3.0">CC BY 3.0</a>. Data by <a href="http://openstreetmap.org">OpenStreetMap</a>, under <a href="http://www.openstreetmap.org/copyright">ODbL</a>.'
    }).addTo(map);
    
```


6. Pull in your geojson data and add it as a marker layer, similar to how we did in Tutorial 3.

```
//Fetch some data from a GeoJSON file
$.getJSON("YOUR_FILE", function(plagueData) {
    // create a marker layer
    var testlayer = L.geoJson(plagueData, {
        pointToLayer: function(feature, latlng) {
            return L.marker(latlng, {
            icon: myIcon
            });
        },
        });
```

7. Add the slider controller. These features can all be changed, see the docs [here](https://github.com/dwilhelm89/LeafletSlider/blob/master/README.md).

```
        sliderControl = L.control.sliderControl({
                position: "topleft",
                timeAttribute: "Date",
                layer: testlayer,
                follow: true
            });

```

8. Add the slider to the map

```
        //Make sure to add the slider to the map ;-)
        map.addControl(sliderControl);
        //And initialize the slider
        sliderControl.startSlider();
    });

```

9. Close the script, body, and html tags

10. Check your map by running it on the local server (if you just open the html document with the browser, the slider will not appear).


11. Go back and add the popups to the geojson layer


```
$.getJSON("data/plaguetimeline.geojson", function(plagueData) {
    // create a marker layer
    var testlayer = L.geoJson(plagueData, {
        pointToLayer: function(feature, latlng) {
            return L.marker(latlng, {
            icon: myIcon
            });
        },
        onEachFeature: function (feature, layer) {
            layer.bindPopup(feature.properties.Year + '<br/>' + feature.properties.Event);
            }
        });
```



