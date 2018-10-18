## Tutorial 9: Web Mapping 2

In the [previous tutorial](8_WebMapping1.md), we created markers using Mapbox GL JS by declaring them explicitly in our code. Our basemap data came from Mapbox's defaults, which in turn come from [OpenStreetMap](https://www.openstreetmap.org). 

In this tutorial, we will create a tileset using external data. Just like QGIS, Mapbox can add layers from vector, raster, and point-based information. Unlike QGIS, which works with maps at a single level of detail, Mapbox converts data to tiles which are delivered to the browser as they are needed. Tiles contain data limited to a bounded geographic area and zoom level, which means not everything needs to be visualized at once. This allows for the kind of interactivity we have come to expect from web maps, and it also allows us to make local calculations about the user's position in relation to data.

### Data munging

To begin, we will go through the familiar process of downloading a dataset and preparing it for use. Navigate to the [NYC OpenData](https://opendata.cityofnewyork.us/) portal, and browse the data catalog for 311 Service Requests.

___
![311 Service Requests]
___

View the data, and filter it by `Descriptor: Rat Sightings` and `Created Date is after 09/01/2018`. This will give us rat sightings in NYC since the beginning of the semester, around 2,631 of them. Export the data as a CSV file, and call it `rat_sightings.csv`.

___
![Rat filter]
___

Mapbox's native data format is called [GeoJSON](http://geojson.org/). GeoJSON is a data format, but it is also javascript code—GeoJSON defines an array of javascript objects that have a set of expected properties that can be interpreted spatially. While CSV files, shapfiles, and other formats can work with Mapbox, they are converted to GeoJSON behind the scenes.

However, CSVs can have formatting ambiguities that Mapbox does not like, and these can cause errors when uploading data. So we're going to convert our data to GeoJSON before uploading to Mapbox in a more forgiving parser. There are many tools for this, but the simplest for our purposes now is to use an [online converter](http://www.convertcsv.com/csv-to-geojson.htm).

Upload your `rat_sightings.csv` file to the converter and let it do its thing. Download the result as `rat_sightings.geojson`.

___
![Converting to GeoJSON]
___


### Mapbox tilesets

Navigate to [Mapbox Studio](https://www.mapbox.com/studio/), log in with your account, and find the `Tilesets` tab near the top right. Here you will see the built in tilesets provided by Mapbox, as well as option to create a new one.

___
![Mapbox tilesets]
___

Click `New tileset`, and then `Select a file`. Choose your `rat_sightings.geojson` file and upload it. It may take a few minutes to upload and process, but you should end up with a new tileset:

___
![Mapbox upload]
___


### Mapbox styles

On its own, a vector tile does not contain any information about how it should be displayed. This is different than a raster tile, which is an image. Just like HTML needs CSS to tell the browser how to make the elements appear, vector tiles need style information to accompany them. 

We're going to create our own Mapbox style. Click on the `Styles` tab in the top right of the page. Choose the option to `Pick a template or upload a style`, and create a map based on the `Dark` template.

___
![Mapbox styles]
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

If you click on `Type` you'll see options to display the data as several different types. It should have defaulted to `circle`. You'll see this displayed on the map, but with just abstract styling. Next to the layer tab, you'll see that the `Select data` tab is currently selected—choose `Style` instead. Here we can adjust the look of the circle marker. Go ahead and experiment with the different options.

___
![Circle style]
___

We now have all the rat sightings called into 311 indicated on the map. At this point, we'll want to save our map style. First, let's rename it. Click on the name of the style, currently `Dark`, and call it `NYC Rats`. On the other side of the interface, there is a button, `Publish`. Do this, and you'll see a dialogue box come up where you can find your `Style URL`. Go ahead and copy this, we'll use it later in our javascript. 

___
![Mapbox rename]
___
![Mapbox publish]
___

We're not finished yet, however. The heatmap is compelling way to present these data, but Mapbox provides some more interesting options. We're going to keep this layer and create another.

Over on the left list of layers, there is an option to duplicate the layer. Do this, select the new layer, and rename it `rat-activity`. 

___
![Rat activity]
___

You'll be in the style tab, but click over to `Select data`. Now modify the type by choosing `Heatmap`. Mapbox will warn us that we'll lose our previous customizations to the layer—but that's ok, because this is a duplicate layer. You might want to zoom in at this point to get a good idea of how it might look when being used on a mobile device. Once again, you should see the abstract-styled heatmap:

___
![Heatmap]
___

Mapbox automatically computes heatmap values based on the points in our sightings—play with the heatmap parameters to fine tune it. The algorithm's default is a bit over-zealous—you might want to set the `radius` value to 20px.

___
![Heatmap style]
___


The heatmap should occlude the sightings markers in the previous layer. That will work just fine for our purposes, so go ahead and publish this map again, and we'll be done with Mapbox Studio for now.


### 



### Deliverables

Make a customized webmap with a set of at least five markers that tells a story about a place in the world. Submit the URL to your github.io site (i.e., https://mygithubusername.github.io/webmap_1).


### Bonus

Curious about rat communication?


______________________________________________________________________________________________________________


Tutorial written by Brian House for Mapping for Architecture, Urbanism, and the Humanities ([Fall 2018](https://github.com/brianhouse/mapping-architecture-urbanism-humanities)).




[311 Service Requests]: Images/webmap2_311_requests.png
[Rat filter]: Images/webmap2_rat_filter.png
[Converting to GeoJSON]: Images/webmap2_converting_geojson.png
[Mapbox tilesets]: Images/webmap2_mapbox_tilesets.png
[Mapbox upload]: Images/webmap2_mapbox_upload.png
[Mapbox style chooser]: Images/webmap2_style_chooser
[Position lock]: Images/webmap2_position_lock.png
[Choosing rat_sightings]: Images/webmap2_choosing_layer.png
[Circle style]: Images/webmap2_circle_style.png
[Mapbox rename]: Images/webmap2_rename.png
[Mapbox publish]: Images/webmap2_mapbox_publish.png
[Rat activity]: Images/webmap2_rat_activity.png
[Heatmap]: Images/webmap2_heatmap.png
[Heatmap style]: Images/webmap2_heatmap_style.png
