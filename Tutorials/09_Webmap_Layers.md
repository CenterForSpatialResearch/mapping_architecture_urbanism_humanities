
1. Set up your dependencies - you will need the leaflet css and js files as well as the jquery js file. You don't need any others. Add the title and style the map. Close the header and open the body. 

2. Open and close a div for the map and open a script tag.

3. Initialize the map, center it and se the zoom level.

```

var map = L.map('map').setView([LAT, LONG], ZOOM);

```

4. Create an empty variable for the layers control. We will add each layer to this as we create it. This will also add our layers to the map.

```
var layerControl;

```

5. Pull in the tile layer and attribute it appropriately.

```
L.tileLayer('http://tile.stamen.com/terrain/{z}/{x}/{y}.png', {
        attribution: 'Map tiles by <a href="http://stamen.com">Stamen Design</a>, under <a href="http://creativecommons.org/licenses/by/3.0">CC BY 3.0</a>. Data by <a href="http://openstreetmap.org">OpenStreetMap</a> and <a href ="https://census2011.adrianfrith.com/">2011 South African Census</a> collected by Adrian Frith'
    }).addTo(map);

```

7. Go back to your QGIS Johannesburg project and calculate percentage of the population for each column you are interested in. Do this by opening the attirbute table, navigating to the field calculator and creating a new field by dividing the target field by the Total. Once you have tabluated all of the columns you are interested in, save your new table by toggling off the Editing Mode. 

8. Save the layer with your new calculations as a geojson.  I'm going to call mine "censusjhb.geojson"

9. Add this geojson to your project. We will use jquery to do this. When we load the geojson, we have to name it's function, we will call ours jhbData.

```
$.getJSON("data/censusjhb.geojson",function(jhbData){

```

10. Ultimately, I want this layer to be a choropleth of the percent of White-identified people in each census tract. I will style by choropleth here using equal intervals. I could (and probably should) use Jenks breaks, which I could calculate in QGIS (or R or another statistical package if you prefer). But, for now, I'll just use equal intervals. 
Because my end goal is to be able to manipulate this layer, I'm going to set this layer equal to a variable befor importing it into my map. 

```
var white = L.geoJson( jhbData, {
style: function(feature){

```

11. In order to style my coloring, I'm going to have to use 'if-statements' for each color. The logic is quite simple, "if this is true, do that", "else if this other thing is true, do something else", etc. I looked up these colors with [ColorBrewer](http://colorbrewer2.org/#type=sequential&scheme=BuGn&n=3).

```
        var fillColor;
        density = feature.properties.PWhite;
        if ( density > .80 ) fillColor = "#006837";
        else if ( density > .60 ) fillColor = "#31a354";
        else if ( density > .40 ) fillColor = "#78c679";
        else if ( density > .20 ) fillColor = "#c2e699";
        else if ( density > 0 ) fillColor = "#ffffcc";
        else fillColor = "#f7f7f7";  // no data
```

12. Now tell it how to style each polygon with a return statement


```
        return { color: "#999", weight: 1, fillColor: fillColor, fillOpacity: .6 };
      },
```


13. We would also like to give users the chance to inspect each region more closely, so we want to add popups to each polygon. We will do that with a bindPopup command.

```
      onEachFeature: function( feature, layer ){
        layer.bindPopup( "<strong>" + feature.properties.Name + "</strong><br/>" + feature.properties.PWhite + " percent of this area is White" )
      }});
```

14. Now we will add our layer to the map.

```
    map.addLayer(white);
    layerControl.addOverlay(white, "White");
  });
```
15. Do the same for the mines, but rather than styling with an if-statement, we will just return one color for all of the mines. 

```
$.getJSON("data/2kmines.geojson", function(minesData) {
	var mines = L.geoJson(minesData, { 
			style: function(feature) {
				return { color: "#000", weight: 1, fillColor: "#daa520", fillOpacity: .6 };
			},
       	onEachFeature: function( feature, layer ){
         layer.bindPopup( "<strong>" + feature.properties.site_name + "</strong><br/>" + feature.properties.commod1 )
       }});
	map.addLayer(mines);
	layerControl.addOverlay(mines, "Mines");
	});

```

16. Now we will set up our layers control. However, this is not working yet, and I'm not sure why. We should be able to make a new object that groups the overlay layers and pass that to Leaflet's L.control.layers function and then add that function to the map. This code does not break the project, but the layers control is not appearing. Until this is fixed, each demographic group's map will have to be a separate map rather than toggle-able layers. 

```
// This is currently not working and I have no idea why not. Currently all layers must be displayed at the same time.

       var overlay = {
         "White": white,
         "Mines": mines
       };



layerControl = L.control.layers(overlay);
map.addControl(layerControl);

```


17. Close the script, body, and html tags. 

18. Look at your map.



The next stage of this project would be to style the map more clearly and make more layers for different ethnic groups. You can change the basemaps, change the colors, style the popups, and explore other Plugins available through [Leaflet](leafletjs.com/plugins.html)

To complete this tutorial, submit your working FOLDER.

______________________________________________________________________________________________________________

Tutorial written by Michelle McSweeney, for Conflict Urbanism: InfaPolitics, a seminar taught at Columbia University by the [Center for Spatial Research](http://c4sr.columbia.edu) in Fall 2017.
