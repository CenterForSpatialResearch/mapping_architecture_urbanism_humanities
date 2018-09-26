## Tutorial 7: Georeferencing

In this exercise, you will explore some of the georeferencing tools available in QGIS and use them to georeference a 1909 map of Mumbai, "Island of Bombay". You will learn how to use GIS tools to georectify raster datasets. You will then use the georeferenced map to digitize vector features from the map infomation. 


## Part I - Georeferencing a scanned paper map

### Notes on the data

The map you will be using for this exercise is Mumbai from 1909 (then Bombay). This is a public domain map, so we can use it freely. The wikimedia record for the map can be found [here](https://commons.wikimedia.org/wiki/File:Bombay_1909.jpg). We are using the highest resolution available (1,571 × 2,104 pixels).

You are going to use [OpenStreetMap](https://www.openstreetmap.org/about) (OSM)  as reference data for the georeferencing process.  OSM provides a free, open-source map of the world from public domain and volunteered data.

### Before you begin
If you haven't already, download the GitHub repository for this course. Using the green button [here](https://github.com/michellejm/ConflictUrbanism-InfraPolitics), select `Download ZIP`. The Data folder will then have all of the datasets needed for tutorials. 

### Setting up QGIS

Open QGIS:

![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef1_.png)

You are going to use OSM data as the reference data for the georeferencing process. You can view the OSM basemap service is QGIS through the OpenLayers plugin.  This plugin does not come pre-installed with QGIS, so you will probably need to add it.  Under the plugins menu, select “Manage and Install Plugins…” 
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef2_.png)

The plugins dialog will open.  Search for “Openlayers Plugin.”  Highlight it, and click “Install plugin”:

![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef3_.png)

It may take a few seconds to install.  Close the plugins menu when finished.  The OpenLayers tools should now appear under the Web menu: 

![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef4_.png)

This plugin will allow you to view a number of basemap services and steam them directly into your QGIS workspace.  Choose the OpenStreetMap>OpenStreetMap option.
Since you are working in a new QGIS project, the map should show the entire earth as the default: 
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef5_.png)
Use the zoom in tool ![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef6_.png) and zoom into Mumbai, India:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef07.png)
Now you will access the georeferencing tools and match the scanned map to the OSM map.  

Under the Raster menu, select Georeferencer>Georeferencer:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef24.png)

The Georeferencer screen will open:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef9_.png)

Click on the Add Raster button ![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef10a_.png) and navigate to the JPEG image "bombay-india-historical-map-1909.jpg" from the class files in the directory ConflictUrbanism-InfraPolitics/Data.  

It will appear in the georeferencer window:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef10.png)

This map is from the years just after the 1896 plague, and while the infrastructure (and even some of the land) have changed, we an still navigate our way around using the docks, bays, and some major roads.

Historical maps can be difficult to georeference, and this sheet poses a number of complications. The map projection is unclear and there are no ground control points or coordinates specified. Because of this, you will georeference by matching physical features represented on the map with their current counterparts (and their known coordinates). However, most of the features in this map have changed or no longer exist (or were never actually built in the first place). Thus, you will need to be very careful to choose locations that you are confident match up well with their contemporary counterparts. Fortunately, some things (such as docks in the East, and some structures have not changes). 

### Adding a Point
The QGIS georeferencer does not allow you to view both the scanned map and the workspace at the same time, so you will have to inspect both maps in turn and choose carefully to select locations to add georeferencing control points. 
One candidate is the Prince's Dock in Bombay Harbor which continues to exist largely its original configuration.  Use the georeferencer zoom tools ![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef11_.png) to zoom to the south-oriented dock: ![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef11.png)
 
Identify as precise a location as possible (a corner of the dock will work nicely) and click on it in the georeferencing window using the add point tool ![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef17_.png) When you do so, the Enter map coordinates window appears:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef14_.png)

If we knew the coordinates of this location, we could now add them manually, but since we do not, we must select them from the OSM data in the main QGIS window.  Click on the ![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef15_.png) button to see the QGIS workspace.  
You may want to use the QGIS zoom tools to zoom in very close to the dock.
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef12.png)

(You may need to reactivate the add button tool ![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef17_.png) by maximizing the georeferencing window and clicking the ![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef18_.png) button again).  

Once you select the same location on the main workspace window (the OSM window), you will automatically be brought back to the georeferencing window where the assigned coordinates will be imputed (you will have different numbers).  
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef19_.png)

At this point, if you are dissatisfied, you can move the assigned control points with the move GCP point tool ![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef20_.png) or delete it entirely and start over with the delete point tool ![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef21_.png) 
If satisfied, click the OK button and the point will be assigned and appear on the map.  Also, a link table entry will be generated on the bottom of the window:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef13.png)

### More points

To add a second point, repeat the same process.  It is a good idea to choose another point in a different portion of the map. Landmarks and major intersections work well for this in order to space out the pins.  
Here, I have zoomed to Jacob's Circle - an intersection where 7 roads (3 major) come together.
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef15.png)


Repeat the same steps as used before to select the center of the same intersection from the OSM map and add the locations to the link table:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef14.png)

You will need to add a minimum of five points to complete the georeferencing (although more is generally better).  Generate at least three more points of your own choosing in different parts of the map and add them to the link table.

Be careful to make sure that the control points you add do in fact match.  This can be quite tricky as many of the features have changed.  

Normally it is a good idea to choose control points from throughout the map.  However, in this case this will be difficult as there are few features in the eastern sections of the map that can be reliably associated with contemporary locations.
In this example, I have selected six control points:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef16.png)

![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef17.png)

It is good practice to save the table of control points that you are building.  To do this, choose “Save GCP points as” under the file menu and save it in the .points format in the same location as the image. This will allow you to later recreate the work you have done:

![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef21.png)

Next, you will “transform” the image and create a georeferenced version of the scanned map image. In the georeferencer window, select transformation settings under the settings window:

![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef20.png)

Here you can select the transformation type (Polynomial 1 should be appropriate here), resampling method (Cubic is often used for resampled images and photos), output location and name, and the spatial reference system (here I have selected EPSG:3857, the pseudo Mercator projection used in the OSM data).  You can also opt to have the georeferenced layer added to QGIS when finished: 

![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef19.png)

Close the settings options and click on the start georeferencing button ![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/GeoRef29_.png).

After the transformation finishes, you should see the map appear in the QGIS workspace:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef17.png)

You can make the scanned map layer partially transparent in the layer properties.  Right click on the map in the layer panel and select properties. On the left panel in the properties dialog, select Transparency, here you can adjust the global transparency with a slider: 
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef22.png)

Compare the georeferenced map with the Open Street Map layer.  Make sure that features appear to match up closely:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef23.png)

In the next section, you will be using the sheet you georeferenced here and digitizing some of the features from it. 

Export your georeferenced layer as a GeoTiff - be sure to select 'Rendered Image' and CRS 4326 (Psuedo Mercator). Rendered image will remove the black background. CRS 4326 because that is what all webmaps use and we will use this map in a webmap next.

To complete Part I, upload the GeoTiff to Canvas.


## Part II - Making Vector Data from Raster Data

#### Premise

In this part of the exercise, you will explore some of the on-screen hand digitizing tools available in QGIS and use them to digitize houses, hospitals and other features from a georeferenced map.  You will convert raster spatial data into vector based features.

#### Notes on the data: 

The map you will be using for this exercise is the 1909 "Island of Bombay" map that you georeferenced in the previous exercise. If you have not already done so please complete the [Georeferencing](https://github.com/michellejm/ConflictUrbanism-InfraPolitics/blob/master/Tutorials/02_Georeferencing.md) exercise. 

### Digitizing Exercise

Open QGIS:

![DigitizingExercise](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/Digitize1.png)

Click on the add raster button ![DigitizingExercise](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/Digitize2.png) and navigate to the georeferenced image you made in the [Georeferencing](https://github.com/michellejm/ConflictUrbanism-InfraPolitics/blob/master/Tutorials/02_Georeferencing.md) exercise.  Since you verified its accuracy already, you will not need any basemap data for this exercise:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef25.png)

This map is from the years after the plague had subsided and the population of the city was back up to nearly 1 million people (977,822). Every structure, road, and rail line is mapped.  In this exercise, you will create new vector datasets, and hand-digitize some residences, transit stations, and hospitals. You will then annotate the map with significant social and political events leading up to and folliwng the plague. 

The red on this map represents private structures. The black represents public structures. The vast majority of private structures at this time (and presently) are housing, though cotton mills and other factories are also red. One of the first things we notice is the large blocks of red in the southern part of the city and the scattering of small red squares across the city. The large blocks of red mostly represent large tenements whereas the small mostly represent houses. As you may expect from a colonial era, housing in Bombay was highly segregated. Likewise, as an island city, there was (and still is) a severe housing shortage for all but the wealthiest.

As we know, maps reflect who made them. This map was made by the British for the British government. Many informal housing arrangements are not represented on this map. 

After the 1896 plague, many people were forcibly moved to the northern parts of the city. Many of these people were poor, and affordable housing was not available. The Bombay City Improvement Trust was supposed to improve the city overall by building apartments, roads, and sanitation. It fell very short of that goal. However, development was started in Dadar, Matunga, Wadala, and Sion in 1899. These areas surround Dharavi. In 1896, Dharavi had a tannery and a small pottery industry. These industries cause a lot of pollution, the CIT did not invest here. Dharavi today is an informal settlement, built primarily by the people who moved there during the plague and those who moved there looking for work in the century since. Today it is one of the largest and most densely populated slums in the world (it is not the largest in the world, Asia, or even India, it is just in a really visible location). 

In this exercise, we will digitize some of the features in the northern area of Mumbai. Because of time, this dataset will not be comprehensive. 

Remove any transparency from the georeferenced map layer. Zoom into the northern area of the island so you can see Matunga, Wahira, and Dharavi:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef26.png)

Next you will create a new point layer.  Under the layer menu, choose create new layer and new shapefile layer:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef27.png)

 In the new vector dialog layer, choose type “Point”.  You can also create additional attribute fields for your dataset, if applicable.  Here, I have added a building “type” attribute and made it a text field with a maximum length of 80.  Select OK:
 
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef28.png)

Save the new file in the same directory with your map, and name it MumbaiBuildings1909.


 The layer will now appear in the Layers panel:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef29.png)

Now begin editing by depressing the toggle editing tool ![DigitizingExercise](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/Digitize9.png) while the buildings layer is highlighted in the layers panel.  Now you can use the add feature tool ![DigitizingExercise](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/Digitize10.png) to start creating buildings.  Click on one of the buildings in the map.  An attribute dialog appears where you can type in attribute information for the feature you just created:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef30.png)

There is no information on the map in regards to the type of building; however, the building I clicked on is labeled as a Fort, so I am entering that information now. It is a good idea to enter this information for any objects that you know their identity (forts, hospitals, train stations, etc.). Click OK when finished. 

Continue to digitize buildings in this corner of Bombay:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef31.png)

If you want to move one of the point features you can use the move feature tool ![DigitizingExercise](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/Digitize13.png) if you want to delete a feature, you can select it with the select features tool ![DigitizingExercise](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/Digitize14.png) at use the delete key on the keyboard.  It is a good idea to regularly use the “save for selected laters” function to save your work as you digitize:  
![DigitizingExercise](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/Digitize15.png)

When finished, depress the toggle editing tool ![DigitizingExercise](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/Digitize16.png) to close the editing session
Next you will digitize the railways.  Create another new shapefile layer as above, but this time choose “Line” as the vector type and MumbaiRailways1909 as the file name:

![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef32.png)

Now when you toggle on editing and select the add features tool ![DigitizingExercise](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/Digitize19.png).  You will be digitizing line features.  As you click with the add features tool you can continue to add as many vertices to the line as you wish.  To complete the line segment, use the right mouse button. 

Line features are more complex than the point features you made earlier and a few more considerations need to be made. One issue regards the shape of the railways (or streets). You are going to digitize the ‘centerline,’ using a single line feature to represent the center of the path feature.  

You also must decide where to begin and end the individual line features.  A common practice is to create individual features between every intersection (or station), ending each feature at the next intersection.  In this way, you can represent the connectivity of the features, essentially modeling a network.  It is important then to make sure that the connecting features are exactly coincident.  You can make sure that you connect features while digitizing by taking advantage of snapping tolerances.  Snapping tools will automatically adjust the digitizing tools and ‘snap’ them to specified features as the cursor gets sufficiently close. 

To set snapping, select “snapping options” under the settings menu: 
![DigitizingExercise](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/Digitize20.png)

Set the snapping options for the current layer to be within 10 map units of a vertex or segment:
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef33.png)

Now the add feature tool will automatically snap to another feature’s vertex whenever the cursor comes within 10 meters.

Digitize the first feature using the add features tool ![DigitizingExercise](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/Digitize22.png). Be careful to keep each vertex as close to the railway as possible.  The more vertices you add the smoother the railroad can be. Right click at the first station. 
![blank](https://github.com/brianhouse/mapping-architecture-urbanism-humanities/blob/master/Images/georef34.png)

Begin the next feature at the endpoint of the first and continue to digitize the railroads. This method can be used to digitize the roads as well, but because of time constraints, we will stop here for this tutorial. If you want or need to digitize the southern section of Bombay, you will need to use the polygon tool to represent the blocks that are filled in, which functions similarly to the points and lines. 


#### Deliverables
To complete Part II, upload your MumbaiBuildings and your MumbaiRailways1909 shapefiles (remember to send all of the files) to Canvas.
______________________________________________________________________________________________________________


Tutorial adapted by Michelle McSweeney for *Conflict Urbanism: InfraPolitics* a seminar course taught at Columbia University in Fall 2017 by the [Center for Spatial Research](http://c4sr.columbia.edu/). Originally written by Eric Glass, for *Mapping for the Urban Humanities*. Edited by Brian House for [Fall 2018](https://github.com/brianhouse/mapping-architecture-urbanism-humanities).


