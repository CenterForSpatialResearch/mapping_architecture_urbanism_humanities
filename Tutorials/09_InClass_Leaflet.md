### About Leaflet

Leaflet is a library for Javascript. You don't have to download javascript because it (like HTML and CSS) is a language that all modern browsers are able to read (and instantly recognize). 
If HTML is the structure of a website, and CSS is the style, JavaScript is the functionality or the interaction. It's what makes websites DO things.

jQuery and leaflet are libraries. You can think of libraries as a set of commands that javascript can draw on. I like to think of it as a book of commands that my program can reference. 

Leaflet is specifically designed to create web maps. We expect certain things from our web maps:

* We expect it to load quickly.
* We expect to be able to view different layers of data.
* We expect it to be customizable.
	
Leaflet handles all of this for us. 


### About Web maps

Web maps are maps on the web (duh). They are different from static maps because while static maps are a complete image, web maps consist of layers and images that are only loaded when the user requests them. We're going to break that down. 

Think of a web map as having a base map and layers of data added to it - just like we made in QGIS. The difference is that a web map is based on tiles. Tiles are the key difference between static maps and webmaps. 
What are tiles?

1. Tiles are a bunch of images (usually 256x256 pixels each). These images are placed side-by-side to make it seem like you're looking at one big map, but really, it's a bunch of little squares.
2. How many tiles you see is based on the Zoom Level.

	1. Zoom level 0 : the whole world in 256x256
	![world](https://github.com/michellejm/ConflictUrbanism_LanguageJustice/blob/master/Images/zoom0.png)
	2. Zoom level 1 : 4 tiles for the whole world
	![world](https://github.com/michellejm/ConflictUrbanism_LanguageJustice/blob/master/Images/zoom1.png)
	3. Zoom level 2 : 16 tiles for the whole world
	3. The further you zoom in, the more tiles are used to make up the whole world.
	4. You usually do not need to look at the whole world, so you only look at some of the tiles at that zoom level. 
	
		1. Most maps showing the entire continental United States are at zoom level 4.
		2. Zoom level 4 requires 256 (4^4) tiles to make up the whole world. But you don't need to look at all 256, you only care about a section of them, so you **zoom** in on the section.
		
	5. Tiles are images. We will use url's to get tiles from other places on the internet. Essentially we will point to
	the site to load the tiles for us. These sites are very stable, so there is little concern that your tiles will disappear. You could, in theory, make your own tiles and load them into your map from a static file. That is a very advanced technique.

## Our Project
We want to make a webmap that shows where the most refugees were resettled in the United States in 2014. We would like to color each state based on how many refugees were settled there, and drop pins on the city giving more information. This will be our starting point for making other webmaps if there is time.

### Collect the ingredients

1. Download the [data and files from the GitHub](https://github.com/michellejm/mapping_arch_urban_hums/tree/master/Data/09_InClass_Leaflet/mymap)
2. Normally, you would export your GeoJSONs from QGIS and find the libraries online.

## Building a Webmap

Begin with setting up a local server.

**PLUGIN for ALL COMPUTERS INSTRUCTIONS**

If running a local server from the command line has not worked for you before or you don't want to go through the Terminal, or you are on a PC, please install [this Web Server plugin](https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb?hl=en) on Google Chrome. This plugin will allow you to view your page as if you were viewing it on the Internet. You MUST use Google Chrome to view your webmap. You will also use Google Chrome for the HTML Tutorial.

When you launch the new app, you will be prompted to select a folder. Choose your mymap folder. Make a note of the address for your local server, in my case below, it is http://127.0.0.1:8887/

![add](https://github.com/michellejm/ConflictUrbanism-InfraPolitics/blob/master/img/wp1.png)

The mymap.html file should appear as a selection. Select that and you will have a blank screen.

**ADVANCED for MAC INSTRUCTIONS**
First you need to make sure you have Python installed. Check if you have Python installed and which version it is. We will be using Python to run a local server.

### (Mac) Check which Python version you have (if any)

1. Open a Terminal Window
2. Type `$ python -V` hit 'Return'
4. Make a note of which Python version you have, you will need it later.

### (Mac) Set up a local server

We will run a local server from our computers. The details of this are far beyond this tutorial, for more on  the technical details, visit the [Mozilla Developer site](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/Set_up_a_basic_working_environment). For more on how the web works the difference between a local and remote server, [read this](https://devdojo.com/blog/technology/local-vs-remote-servers).

1. In a Terminal window, navigate to the folder where you have saved your html file (directions below on how to "navigate"). In my case it is in Documents > MappingForTheUrbanHumanities_2017 > Class_Data >3_Webmaps. To navigate there, I type the following commands (don't type the $, that just indicates that you are in the Terminal):

	* `$ cd Documents`
	* `$ cd ConflictUrbanism-Infrapolitics`
	* `$ cd Class_Data`
	* `$ cd 3_Webmaps`

2. If you have Python 3, type:

	* `$  python -m http.server`  

2. If you have Python 2, type:

	* `$ python -m SimpleHTTPServer`

3. Return to your browser window (Chrome, Firefox, or Safari) and type `localhost` in the navigation bar. You should see an empty webpage.

### Check to see if it worked

3. Return to your browser window (Chrome, Firefox, or Safari) and type `http:\\localhost` in the navigation bar (you may have to type `http:\\localhost:8000` or copy/paste from the app's location). You should see an empty webpage.



# Initialize the map

### Write the HTML Document

You have a handout with the complete code. You can see it [here](https://github.com/michellejm/mapping_arch_urban_hums/blob/master/Data/09_InClass_Leaflet/mymap/index.html). We are going to go line-by-line. The code is explained and then provided in sections after each explanation. 

We will begin with the *Header*

1. Import the stylesheets

	1. Import the stylesheet with this language: 
	
	`<link rel="stylesheet" href="css/STYLESHEET.css">`
	
	2. We will use 3 stylesheets:
	
		1. leaflet.css
		2. font-awesome.min.css 
		3. font_awesome_markers.css 
		
2. Javascript

	1. Import the Javascript files with this language: 
	
	`<script src="js/NAME.js"></script>`
	
	2. We will use 3 javascript scripts:
	
		1. leaflet.js
		2. jquery-2.1.1.js
		3. awesome_markers.js
		
3. Set the size of the map

	1. This works by indicating the style of the map in a way the browser can interpret
	
	`<style> #map {width: SIZEpx; height: SIZEpx; } </style>`

Type this into your document exactly (DO NOT copy/paste. [Click here](https://medium.freecodecamp.com/the-benefits-of-typing-instead-of-copying-54ed734ad849#.ksfgl7p86) for a short rationale for ALWAYS typing your code).

```
<html>
<head>
  <title>A Leaflet map!</title>
  <link rel="stylesheet" href="http://cdn.leafletjs.com/leaflet-0.7.3/leaflet.css"/>
  
  <link rel="stylesheet" href="//maxcdn.bootstrapcdn.com/font-awesome/4.2.0/css/font-awesome.min.css" >
  <link rel="stylesheet" href="css/leaflet_awesome_markers.css">

  <script src="http://cdn.leafletjs.com/leaflet-0.7.3/leaflet.js">  </script>
  <script src="js/jquery-2.1.1.min.js">  </script>
  <script src="js/leaflet_awesome_markers.js"></script>

  
  <style>
    #map{ width: 900px; height: 500px; }
  </style>
</head>
```

### Body - start the map

1. Start the body
2. Make a section (div) for the map, and call it map.
3. Start a javascript command by writing `<script>`
4. Initialize the map by creating a map variable

	1. Javascript requires that all variable be labeled with 'var' we are going to create a map var and use leaflet to initialize it. We need to give out map two parameters. First, the latitude/longitude, and the zoom level. 
	2. set `var map =`
	3. When we call leaflet, we use capital L  `L.map`
	4. Js (and therefore Leaflet) uses both dot notation and bracket notation, whenever there's a period between things in js, it's called dot notation, and you are "accessing the properties" of an "object" don't worry too much about this - it just means you are looking inside of that object to find a particular property.
	
		1. lat/long = [39.76, -98.50] is about central in the US, and zoom level 4 is good for viewing the entire area. We call Leaflet's map property, pass it the string, 'map', setView at ([lat/long], zoom)

5. Add a tile layer

	1. Call the tileLayer property from Leaflet, and pass it the web address of your tiles.
	
		1. We are going to use Open Street Map "http://{s}.tile.openstreetmap.org"
		2. Tile layers have a location and 3 variables: http://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png
		
			1. "http://{s}.tile.openstreetmap.org" = name of the tile server
			2. z = zoom level
			3. x/y - latitude & longitude 
			
	2. You must pass the tileLayer at least one property (we will give it 3 )
	
		1. The attribution - for the tiles and the data. This goes in curly brackets after the tile address.
		2. Set the min and max zoom levels (I'm using 1 (the whole world), and 12 (about 1:150,000 [see here](http://wiki.openstreetmap.org/wiki/Zoom_levels)
		
	3. Add it to the map
	
		1. Pass the .addTo property, and give it the map variable.
	
```
<body>

  <div id="map"></div>

  <script>

  // initialize the map
  var map = L.map('map').setView([39.76, -98.50], 4);  

  // load a tile layer
  L.tileLayer('http://{s}.tile.openstreetmap.org/z}/{x}/{y}.png',
    {
      attribution: 'Tiles by <a href="http://www.openstreetmap.org/">OSM</a>, Data by <a href="http://census.gov">US Census</a>',
      maxZoom: 12,
      minZoom: 1
    }).addTo(map);
 
   	// Be sure to keep these closing tags at the end of your document
 	</script>
	</body>
	</html>
```

6. Let's go check it out on our website. Go back to the web browser at localhost:8000. A basic map of the US should appear. 

Congratulations on starting your webmap!


### Body - make the map

1. First add the location of the cities

	1. Use the jQuery command, getJSON. Use $ to call the jQuery library, just like Leaflet is **called** with L., jQuery is **called** with $.
	2. Tell jQuery where your file is located, and give the function a name (I'm going to use 'data'). 
	3. Just like with the tiles, we want to add this element to the map.

```
// load GeoJSON file
  $.getJSON("data/FILENAME.geojson",function(data){
    L.geoJson(data).addTo(map);
    });  
```

2. Inspect the map.
3. We can do better than blue pins, we will change the icons

	1. There are two options for using different icons.
		1. You can upload a file to use as an icon with the iconUrl function.
		2. You can use font awesome as a **plugin** to Leaflet to pull in all the [Font Awesome](http://fontawesome.io/get-started/) icons. 
	2. We will use the font awesome package because it has so many options for different icons
	
4. Define the marker

	1. Specify a variable for our markerUse `var` to define the variable. In the beginning of the curly brackets, after function(data). I'm going to call this marker 'aMarker'
	2. Call Leaflet with L and call the AwesomeMarkers.icon property. This syntax is from the Awesome Markers [documentation](https://github.com/lvoogdt/Leaflet.awesome-markers)
	3. There are a few other icon sets, so we have to set the prefix as 'fa', find the icon you want (we're going to use 'home' to represent Refugee Homes), and tell it what color to make the icons (I think black is easiest to see).
	
```  
	$.getJSON("data/FILENAME.geojson",function(data){
    var aMarker = L.AwesomeMarkers.icon({
       prefix: 'fa', //font awesome rather than bootstrap 
       icon: 'home', 
       iconColor: 'black',
    });
```

5. Now add the marker to the map

	1. Use the Leaflet library's geoJson function, and pass it the data you loaded earlier
	2. Give it the function of pointing to a layer feature at the lat/long. 
	3. Tell it to return the marker we made at a given latitude and longitude. 
	4. Add it to the map.
	
```  
$.getJSON("data/FILENAME.geojson",function(data){
    var aMarker = L.AwesomeMarkers.icon({
       prefix: 'fa', //font awesome rather than bootstrap 
       icon: 'home', 
       iconColor: 'black',
    });
    *****NEW*****
L.geoJson(data,{
     pointToLayer: function(feature, latlng){
     	return L.aMarker(latlng,{icon: aMarker});
     }
    }).addTo(map);
});
```

6. Add Interaction!! This is the real power of web maps. Let's make each house icon a popup showing the city name and how many refugees were settled there in 2014.

	1. After the point to layer function, we need to define another variable. I'm goign to call it 'marker'.
	2. Add the 'bindPopup' function to the marker
	3. Pass the bindPopup the names of the columns you want to use. 
	
		1. Use the feature.properties.COLUMN_NAME format
		2. We want both City_state and Individuals the <br/> adds a line between the two rows.
		3. This should be a functional map that you can click on houses and get a popup about that house. 
 
```
    L.geoJson(data,{
    pointToLayer: function(feature,latlng){
    
    ***** NEW *****
    	var marker = L.marker(latlng,{icon: aMarker});
    	marker.bindPopup(feature.properties.City_state + '<br/>' + feature.properties.COLUMN_NAME);
    	return marker;
    	}
    ***** END NEW *****
    
    }).addTo(map);

});
```

7. Add polygons!! This works the same way that points do. In this example, we will use the tallied refugee data by state to make a chloropleth map. 

	1. Add the state data in the exact same way you loaded the cities data. 
	2. Call jQuery with the $, and pass it the location of the state data and add it to the map
	3. It doesn't matter (for this example) what layer is added first. It would if we were layering multiple polygons, it would matter which one was on top, though.
	
	```
	$.getJSON("data/FILE_NAME2.geojson",function(stateData){
  		L.geoJson(stateData).addTo(map);
	});

	```
	
	4. Style the fill of each polygon based on the number of refugees in the area. Look to the data for natural breaks in the data. *For example, there are 404 individuals in Georgia, and 386 in North Carolina. Since the next closest number is 308 in Indiana, it makes more sense to group NC in with GA. It is important to get a feel for the data before assigning these breaks in order to accurately represent what the data can illustrate.* 
		1. Pick your colors, we are going to use 5 different groups (plus a "no refugees" group). Color matters, lighter shades should be fewer people, darker should be more. Really, we should reserve chloropleths for density scales, but since we are focusing here on individuals rather than density, we will use the raw numbers [ColorBrewer](http://colorbrewer2.org/#type=sequential&scheme=YlOrRd&n=5) makes finding appropriate hex codes easier.
	
8. This map is almost impossible to read with all of those houses clustered together. It would be much better if we grouped them together and they spread out when we zoomed in. There is a package for that, it's called [Marker Cluster](https://github.com/Leaflet/Leaflet.markercluster/blob/master/README.md). The power of Leaflet is that it is Open Source and there is a package for almost anything you could want to do. We are going to use the Marker Cluster package in our map.  

	1. Download [Marker Cluster](https://github.com/leaflet/Leaflet.markercluster/downloads).
	2. Move the MarkerCluster.css file to your css folder.
	3. Move the MarkerCluster.Default.css to your css folder.
	4. Move the leaflet.js file into your js folder.
	5. Each of these stylesheets and the js file need to be referenced in the header.
	
		1. Use the same format as we did for the font awesome files. 
	
	```
	<head>
	<title>A Leaflet map!</title>
  	<link rel="stylesheet" href="css/leaflet.css"/>
  	<link rel="stylesheet" href="css/MarkerCluster.css">
  	<link rel="stylesheet" href="css/MarkerCluster.Default.css" IS SO>
  	<link rel="stylesheet" href="css/font-awesome.min.css >
 	 <link rel="stylesheet" href="css/leaflet_awesome_markers.css">
  
  	<script src="js/leaflet.js"></script>
  	<script src="js/leaflet.markercluster.js"></script>
  	<script src="js/jquery-2.1.1.min.js"></script>
  	<script src="js/leaflet_awesome_markers.js"></script>
  
  	<style>
	```
	
	6. Include the groups
	
		1. Change the command where we add the new data to be a variable.
		2. Add another variable, "clusters", and set it equal to the Leaflet markerClusterGroup() function. No need to pass it any properties yet.
		3. Add the clusters layer with the new rfg ClusterLayer
		4. Add it to the map
		5. Remove the previous "add to map" function
		
	```
 	***NEW****
	var rfg = L.geoJson(data,{
	*** END NEW ***

    	pointToLayer: function(feature,latlng){
    	var marker = L.marker(latlng,{icon: aMarker});
    	marker.bindPopup(feature.properties.City_state + '<br/>' + feature.properties.Individuals);
    	return marker;
    	}
    	}); 
    	***NEW***
   	 var clusters = L.markerClusterGroup();
    	clusters.addLayer(rfg);
    	map.addLayer(clusters);
  	});
  	*** END NEW***
	```

9. Those groups are awfully large and the colors don't match our blue. We'd like to limit the area covered by each grouping and change the marker colors. 

	1. From reading the Docs, we know that the maxClusterRadius is 60 pixels (this is always in pixels). We want to set it to 30 pixels instead. To override the default, add the maxClusterRadius property in the markerClusterGroup.
	
	```
	var clusters = L.markerClusterGroup({
    	maxClusterRadius:30
    });
	```

	2. To change the marker colors, find the MarkerCluster.Default.css file (it should be in your css folder). Open it with a text editor. There are 3 marker sizes - Small, Medium, Large. I like to have these in the same color family (i.e., light, medium, dark), and use ColorBrewer to find the RGB codes. Simply change the numbers to the colors you want. The fourth number (.6) refers to opacity. You can change that, too if you need to. 
	
	![markercolor](https://github.com/michellejm/ConflictUrbanism_LanguageJustice/blob/master/Images/markercolor.png)
	
That's it for *styling* our web map (we still have to get it into the page). This should give you a foundation to be able to use some of the wide array of plugins available through Leaflet. The options are nearly endless, but as an example, you can:

* allow users to toggle between [base layers](http://leafletjs.com/reference.html#control-layers), 
* make [layer groups](http://leafletjs.com/examples/layers-control/), 
* add font-awesome [icons as buttons](http://danielmontague.com/projects/easyButton.js/v1/examples/),
* allow users to find addresses with [geocoding](https://github.com/smeijer/L.GeoSearch),
* make a [timeline slider](http://dwilhelm89.github.io/LeafletSlider/)
* and [MUCH MORE](http://leafletjs.com/plugins.html#user-interface).


**Add the map to the page**

You can add your map 2 ways. 
1. Insert it with an iframe. To do this, you will direct the html to your FOLDER where you have been saving everything. Ideally, you will move the ENTIRE folder we made here (possibly rename it to something more logical) and set the src to the folder. Inside the folder, your html file should be called index.html (as we did here). 

```
<iframe src="path to the folder"> </iframe>
```

2. Insert an image into your page that links to the map. For example, 

```
<a href="mymap/index.html"><img src="img/skyline.jpg" class="img-responsive"/></a>
```


That's it! Congratulations! You have made a webmap with Leaflet!

If we have time, we will work on Advanced versions


__________________________________________________________________________________________

This tutorial was prepared by Michelle McSweeney for Mapping for Architecture, Urbanism and the Humanities, offered by the [Center for Spatial Research](http://c4sr.columbia.edu) at Columbia University in Fall 2017. 


