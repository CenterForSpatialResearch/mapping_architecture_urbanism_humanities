## Annotation and Digital Storytelling with Leaflet.js

With this exercise you will learn how to create a basic web-based map using the javascript library leaflet.js. This is necessarily not an introduction to javascript, but rather intends to provide familiarity with the components and requirements of publishing maps online. Upon completing this tutorial you should ....

1. Understand how to export GeoJSON files from QGIS
2. Understand the structure of a basic webpage
3. Be able to import a historical map to be shown on the web
4. Create interactive vector features with embedded photos and annotations
5. Experiment with layer styles and map composition


#### Notes on the data:

You will use the 1909 "Island of Bombay" map that you georeferenced in the the [Georeferencing](https://github.com/michellejm/mapping_arch_urban_hums/blob/master/Tutorials/06Georeferencing.md) exercise. Alternatively, you can use [this map](https://github.com/michellejm/mapping_arch_urban_hums/blob/master/Data/6-7_Georeferencing-Annotation/data/refmum.tif), click on 'View Raw' to download.

You will also need a list of events surrounding the plague. [This list](https://github.com/michellejm/mapping_arch_urban_hums/blob/master/Data/6-7_Georeferencing-Annotation/data/plague_timeline.csv) was compiled from a variety of sources. Some events include links to images as well. You can click 'View Raw' and then RightClick and SaveAs a CSV.

#### The Premise

In this exercise, you will annotate your map with data we have collected about the plague events in Bombay. Using the GeoTiff and a spreadsheet of events from the years before and after the plague, you will create a timeline telling the story of how the plague started a series of events that amplified the housing shortage in Bombay and gave the colonial powers the opportunity to gain more control over individuals' lives.

For context lets take a look at what this final map will look like. It is visible [here](https://michellejm.github.io/ConflictUrbanism-InfraPolitics/Data/3_AnnotationWebMap/)

To accomplish this we will be using Leaflet.js, a well documented javascript library with many online easily accessible examples. Review the [Docs](http://leafletjs.com/reference-1.0.3.html) for explanations of all of the functions in the leaflet library.

Leaflet.js is one of many many ways to create webmaps. Others include:
* OpenLayers
* Neatline (a plugin for Omeka)
* Mapbox
* Carto
* TileMill
* GeoServer

Should you look into creating webmaps further you'll fine that many of these tools are used together, or build upon each other.

### To Begin

We have provided the basic folder structure that you will need to create your webmap, in the `Data/6-7_Georeferencing-Annotation` folder downloaded with this repository. Lets review the structure before we proceed. It contains three folders and one file:

* `css`
* `data`
* `img`
* index.html
* `js`

In the broadest terms, web development can be understood in the following way: HTML is the structure of a website, CSS is the style, and JavaScript is the functionality or the interaction. Each of these are contained in text files which we have organized into the above folder structure. When you are creating a webpage you are creating a series of linked text files that your browser reads.

In addition there are folders for images: `img` and `data`. These two folders will contain any images you will use, and the data for the vector annotations as well as the georeferenced historical map.

#### Preparing your data
For the purposes of this tutorial we have already prepared all of the data for you. However the following steps outline how you would go about exporting data from QGIS to include in an online map if you were starting from scratch.

**Exporting and Formatting a Georeferenced Historical Map**
1. *Find the latitude and longitude* coordinates of your image. We will need these later on when we import the historical map into our web map. Enable the `Lat Lon Tools` plugin in QGIS by selecting "Manage and Install Plugins" under the "Plugins" dropdown and searching for "Lat Lon Tools." Select and click "Install Plugin."

![blank](https://github.com/michellejm/mapping_arch_urban_hums/blob/master/Images/georef3-1.png)

Then use it to select the **top left** and **bottom right** corners of your raster and copy these into a text document. You must choose the corners of your raster dataset (i.e. the **corners of the black rectangle** around your rotated map).

![blank](https://github.com/michellejm/mapping_arch_urban_hums/blob/master/Images/georef3-3.png)
2. *Set No Data Value.* Open the `Properties ` menu for the georeferenced historical map. In the Transparency tab enter 0 in the `Additional no data value` field. Click **Apply**. You should notice that the black border around the outside of your image disappears.

![instructions image](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities_2017/blob/master/Tutorials/Images/Webmaps/01_NoData.png)

3. *Export Historical Map as a GeoTiff.* Right click on the georeferenced map in the layers panel and select Save as. Then in the save as dialog box select `Rendered image` as the output mode. Select `GTiff` as the format, and make sure the CRS is set to `Project CRS (EPSG:4326 - WGS 84)`. Name  your image and save it in the appropriate directory. (You must select `Rendered image` in order to export a version of your map without the black border)

![blank](https://github.com/michellejm/mapping_arch_urban_hums/blob/master/Images/georef3-2.png)

4. *Reducing the File Size of the Map Image*. To make it more manageable to load in the browser open the GeoTiff file we just created in the photo editor of your choice, adjust the image size to something more manageable and then export as a **.png**. Save this file in the `data` folder of your webmap directory.

**Exporting Vector Layers as GeoJSON**
We want to layer data that we have collected about events in Bombay layered over our historic map. These points rely on a different data format . Up until now we have been working with shapefiles. When we upload our annotations to the web we will be using a data format called GeoJSON. It is a different way to structure spatial and tabular information and it preserves all of the information we have come to expect from shapefiles.

1. *Upload Annotation File* Click on the 'Add Delimited Text Layer' button ![blank](https://github.com/michellejm/mapping_arch_urban_hums/blob/master/Images/georef3-4.png) and find the plague_timeline file

![blank](https://github.com/michellejm/mapping_arch_urban_hums/blob/master/Images/georef3-5.png)

The points should appear on your map

![blank](https://github.com/michellejm/mapping_arch_urban_hums/blob/master/Images/georef3-6.png)

1. *Export as GeoJSON*. Right click on the layer you want to export in the layers panel. Select Save as. Select `geoJSON` as the file format. Select `WGS 84` (a geographic coordinate reference system) as the CSR. Name  your file and save it in the appropriate directory.
![blank](https://github.com/michellejm/mapping_arch_urban_hums/blob/master/Images/georef3-7.png)

## Building a Webmap

Lets begin to make our webmap.


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

### Deconstructing The Webmap

We will break down the example map provided in the `index.html` file line by line. The full code for the example is also provided at the end of this tutorial.

1. *open* the index.html file in the text editor of your choice (I suggest Sublime or Text Wrangler). Text Edit or Notepade are OK, too, but you won't get the helpful colourings, autofill, etc.

#### Overall Structure

HTML documents are constructed through using what are called tags that are denoted like this `<html> content </html>. All HTML documents follow a basic structure with a series of nested elements labeled with tags.

```html
<html>
<head>
	//this is a comment
	//all of your header information goes in the header,
	//such as the leaflet library and the title of your webpage
	<style>
		//style tags are a way to embed simple CSS styles within your html document,
		//that specify how certain elements on the page should look
	</style>

</head>

<body>
	//all of the content of your webpage is contained within the body tags
	//Think of tags as a set of parentheses that we must both open and then close.
	//Closing tags look like this: </body>
</body>
</html>
```

#### Writing the Header

As mentioned above we are using the Leaflet.js javascript library to create a webmap. Javascript is a language, like HTML and CSS, that all modern web browsers understand naturally.

Leaflet is a set of commands built with javascript that allow us to make webmaps relatively easily.

We will also use another library called jQuery.js that will allow us to import the points and road outlines data.

It is helpful to think of these libraries as sets of commands that our program can draw on.

Because Leaflet and jQuery are both libraries we need to include them in our html document so that they can be referenced by our program when it runs in the browser. These libraries are really just also sets of text files, contained in javascript and css file types.

The included index.html is already written out for you, but below is a walkthrough of what each line is doing.

We include them in the header section: `<head>`

1. *Download required libraries.* We have already done this step for you. But for reference we downloaded the Leaflet.js library from [here](http://leafletjs.com/download.html). And we downloaded the jQuery library from [here](http://jquery.com/download/).  
2. *Name your map.* The `<title>` tag contains the name of your map that will appear at the top of the browser window.
3. *Import Leaflet's CSS.* Leaflet comes with its own CSS styles that specify how certain elements, like the zoom buttons, should look. We have placed this css file in the `css` folder and now need to tell our index.html document where to find it.
	`<link rel="stylesheet"  href="css/leaflet.css"/>`
4. *Import Javascript libraries.* Now we need to import the Leaflet and jQuery javascript libraries. We have likewise saved these in the `js` folder in our webmaps directory.
	`<script src="js/leaflet.js"></script>`

	`<script src="js/jquery-3.2.1.min.js">  </script>`
5. *Set the size of your map.* Using CSS syntax we will set the size of the map to fill a full browser window:
	`<style> #map{position: absolute; top:0; bottom:0; left:0; width: 100%;} </style>`
6. *Close the `<header>` tag.*

#### Writing the Body

Now we will move on to building the content of our map.
Please note: `//` in front of a line means that the code is "commented out" and will not be read by the browser. As we go along, uncomment the code section by section, meaning, remove the `//` from in front of each line, so that the browser reads it. There are some lines that should stay commented, however — if it reads like plain english (e.g. `//link to the URL of the georeferenced historical map image`), leave the `//` comment tags. (If you are unsure, the full, final code is listed at the end of this tutorial)

1. *Open the <body> section.* `<body>`
2. *Make a section for the map.* Make a `<div>` for the map, and call it map.
3. *Start a javascript command.* Write `<script>` to start writing in javascript within our index.html file.
4. *Initialize the map* by creating a map variable.
`var map = L.map('map').setView([18.966, 72.831], 12)`
	* Javascript requires that all variables be labeled with 'var'.
	* We are going to create a map var and use leaflet to initialize it. We need to give out map two parameters. First, the latitude/longitude for the center of our map, and the zoom level.
		1. set `var map =`
		1. When we call leaflet (i.e. access the leaflet library's set of commands), we use capital L  `L.map` to call Leaflet's map property. Javascript (and therefore Leaflet) uses both dot notation and bracket notation, whenever there's a period between things in js, it's called dot notation, and you are "accessing the properties" of an "object" don't worry too much about this - it just means you are looking inside of that object to find a particular property.
		1. `.setView`: We set the view of the map by specifying the center point in latitude and longitude `[18.966, 72.831]`, as well as the zoom level: `12`. In total: `var map = L.map('map').setView([18.966, 72.831], 12);`

##### Add Map Tiles

1. *Add background tile layers.* We are going to be using background tiles from [Open Street Map](http://wiki.openstreetmap.org/wiki/Tiles#cite_note-1) that have been styled by [Stamen Design](http://maps.stamen.com/#terrain/12/37.7706/-122.3782). There are a number of open source map tile providers out there and Stamen has some great versions.
1. *Call the tileLayer property* from Leaflet, and pass it the webaddress of the map tiles we are using. `L.tileLayer('http://tile.stamen.com/toner-lite/{z}/{x}/{y}.png'`
	* We could try switching to a different tile provider `'https://maps.wikimedia.org/osm-intl/{z}/{x}/{y}.png'`
2. *Pass the tileLayer some properties.*
	1. Inside curly brackets after the address for the map tiles we will set the attribution (which is displayed in the bottom right corner). `attribution: 'Tiles from <a href="http://www.openstreetmap.org/">OSM style by Stamen Design</a>',`
	2. We will also set the minimum and maximum zoom levels. `maxZoom: 19,
		minZoom: 1`
3. *Add the tileLayer to the map.* Pass the .addTo property, and give it the map variable. `.addTo(map);`

```javascript
L.tileLayer('http://tile.stamen.com/toner-lite/{z}/{x}/{y}.png',
		{
			attribution: 'Tiles from <a href="http://www.openstreetmap.org/">OSM style by Stamen Design</a>',
		maxZoom: 19,
		minZoom: 1
		}).addTo(map);
```
5. *Save* your index.html document. Open your browser and refresh the `localhost:8000` page. You should see the following: ![img](https://github.com/michellejm/mapping_arch_urban_hums/blob/master/Images/georef3-14.png)

##### Add GeoreferencedHistorical Map

We will now embed the "Map of the Island of Bombay" published in 1909 that we georeferenced in the previous exercise.

1. *Create a variable* that contains the url of the image, in this case the path to where is it stored in the directory for our webmap.
`var imageUrl = 'data/referenced_mumbai.png';`
2. *Create another variable* where you define the area that the image covers using two pairs of latitude and longitude coordinates for the top left and bottom right corners of your image. We found these coordinates using the `lat lon tools` plugin for QGIS when we were preparing the historical map raster earlier.
`var imageBounds = [[19.1012196354, 72.7703713242], [18.8743642027, 72.9320741694]];`
3. *Call Leaflets .imageOverlay()* method pass it the two variable we just created, set the opacity of the map layer and add it to the map.
`L.imageOverlay(imageUrl, imageBounds, {opacity: 0.8}).addTo(map);`
4. *Save and then refresh your browser* and the 1909 map should appear.

##### Define a new marker for the map

1. Make a new variable and call it 'myIcon' and set it equal to a leaflet icon element with the properties
	* iconUrl (where to find the little picture)
	* iconSize (how big it is in pixels)
	* popupAnchor (where the popup appears from)
2. The icon we are suing comes from the [Flaticon](https://www.flaticon.com/free-icon/maps-mark_10030) Collection.
```
var myIcon = L.icon({
iconUrl: 'css/images/maps-mark.png',
  iconSize: [20, 20],
  popupAnchor: [0, -28]
});
```

##### Add Points of Interest (or any geojson file)

Now we will add the file of points related to the plague.

1. *Use the jQuery command, getJSON.* Use $ to call the jQuery library, just like Leaflet is **called** with L., jQuery is **called** with $.
2. *Tell jQuery where your file is located,* and give the function a name (I'm going to use 'plagueData').
3. *add points to the layer* using the latlng category of your geojson
4. *tell the point to produce the marker that you defined in the last step*
5. *Add onEachFeature* as a placeholder for the popup function we will define shortly. Comment this out with // until the next step.
5. *Add the sites to the map*
6. *Save and then refresh your browser,* the sites should be visible.

```javascript
  	$.getJSON('data/plaguetimeline.geojson',function(plagueData){
	    	L.geoJson(plagueData, {
	    	pointToLayer: function(feature, latlng) {
		return L.marker(latlng, {
		icon: myIcon});
	},
	    	onEachFeature: onEachFeature,
	    	}).addTo(map);
	    });

```

##### Add Interactivity to Points (or any geojson file)
We will now add popups for each of our road lines.

1. *Call the leaflet function* `onEachFeature`. This must be defined for two parameters:  `feature` and `layer`. Whatever information we include in this function will be called once for each feature (basically each row) in our GeoJSON file.
3. *Define the action* we want called for each feature:
	* If the plaguetimeline.geojson file has properties (attribute information) and has a column named `Event`
	* Inside that variable store the value of the `Event` column in the plaguetimeline.geojson file.
	* Use the `.bindPopup()` leaflet method to attach the content of the roadsPopup to each element in the plagueData layer
4. *Call the onEachFeature function we just defined.* Return to the block of code above where we added the data and uncomment
5. Use the Leaflet function geoJSON, pass it the features property and the onEachFeature function we just defined.
6. Add it all to the map.


```
//define popup content
function onEachFeature(feature, layer) {
    if (feature.properties && feature.properties.Event) {
        layer.bindPopup(feature.properties.Year + '<br/>' + feature.properties.Event);
    }
}
L.geoJSON(geojsonFeature, {
	onEachFeature: onEachFeature
}).addTo(map);
```
6. *Save your file and refresh* your browser.
![img](https://github.com/michellejm/mapping_arch_urban_hums/blob/master/Images/georef3-8.png)



The full completed code for this example is available here:

```
<html>
<head>
	<title>GeoreferencingMumbai</title>

	<link rel="stylesheet"  href="css/leaflet.css"/>

	<script src="js/leaflet.js"></script>

	<script src="js/jquery-3.2.1.min.js">  </script>

	<style>
	#map{position:absolute; top:0; bottom:0; left:0; width: 100%;}
	</style>


</head>
<body>
	<div id="map"></div>

	<script>
//initializing the map
	var map = L.map('map').setView([18.966, 72.831], 12);

//add background tile layers
	L.tileLayer('http://tile.stamen.com/toner-lite/{z}/{x}/{y}.png',
		{
			attribution: 'Tiles from <a href="http://www.openstreetmap.org/">OSM by Stamen Design</a>',
		maxZoom: 19,
		minZoom: 1
		}).addTo(map);

//link to the historical map image
	var imageUrl = 'data/referenced_mumbai.png';

//define the area that image covers
	var imageBounds = [[19.1012196354, 72.7703713242], [18.8743642027, 72.9320741694]];




//add georeferenced historical map
	L.imageOverlay(imageUrl, imageBounds, {opacity: 0.8}).addTo(map);

//define new marker
var myIcon = L.icon({
iconUrl: 'css/images/maps-mark.png',
  iconSize: [20, 20],
  popupAnchor: [0, -28]
});


//load GeoJSON file containing sites, and style points
  	$.getJSON('data/plaguetimeline.geojson',function(plagueData){
	    	L.geoJson(plagueData, {
	    	pointToLayer: function(feature, latlng) {
		return L.marker(latlng, {
		icon: myIcon});
	},
	    	onEachFeature: onEachFeature,
	    	}).addTo(map);
	    });


//define popup content for site annotations
function onEachFeature(feature, layer) {
    if (feature.properties && feature.properties.Event) {
        layer.bindPopup(feature.properties.Year + '<br/>' + feature.properties.Event);
    }
}

L.geoJSON(geojsonFeature, {

	onEachFeature: onEachFeature
}).addTo(map);

	</script>
</body>
</html>


```

Submit your working FOLDER to courseworks.

______________________________________________________________________________________________________________

Tutorial written by Michelle McSweeney for *[Conflict Urbanism: InfraPolitics](https://github.com/michellejm/ConflictUrbanism-InfraPolitics)*. Adapted from the tutorial written by Dare Brawley, for *Mapping for the Urban Humanities*, which was based on the tutorial prepared by Michelle McSweeney for the Spring 2017 course, *Conflict Urbanism: Language Justice* offered at Columbia Univeristy through the Center for Spatial Research.
