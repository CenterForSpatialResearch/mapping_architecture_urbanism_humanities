## Tutorial 3: Making Data

In this tutorial, you will:
* Download 311 data for New York City
* Create a categorical and a quantitative map of this data.
* Join the 311 data to another spatial dataset (census block groups) to aggregate it and display it based on its geographical location

There are three basic steps in this process:
1. Select, filter and download 311 data
2. Add the 311 data to a map in QGIS
3. Classify the data so that it is correctly displayed on the map.


### Datasets:
The main dataset we will be using in this tutorial is based on 311 data. 311 is a service provided by the city of New York where people can submit complaints or ask questions about living in the city. Some of the most popular complaints filed through 311 are about parking, noise, garbage, rodents, dead or damaged trees, air quality, construction permits, graffiti, homelessness, street conditions, taxis and water quality. There are other categories and each one of these has it's own subcategories. Furthermore, each entry in the dataset comes with the following fields (amongst others):
* Created date
* Closed date
* Agency
* Complaint type
* Descriptor
* Location type
* Incident zip code
* Incident address
* Street name
* City
* Landmark
* Facility type
* Status
* Due date
* Resolution description
* Community board
* X and Y coordinates
* Latitude and longitude

The dataset is a great resource for anyone studying New York. **Nevertheless, a word of caution is necessary**: many people use this dataset to describe and analyze conditions in New York; however, the 311 data doesn't describe the city, it describes the complaints people file, it is not about the city, it is about the complaints, and even though the complaints might tell us something about the city, the distinction is crucial. Every dataset has its own biases and the 311 dataset has very strong ones: it collects data ONLY about the people who complain and ONLY about what they choose to complain about. Again, this dataset is much more about the complaints and the people who complain than about the conditions in the city. There is no 1 to 1 relationship between the 311 complaints and the conditions in the ground. That being said, though, it is still a great resource and very fun to play with. You can find out more about the 311 service [here](http://www1.nyc.gov/311/).

Other datasets we will be using are:

**Most** Are available in the [Data/3_MakingData folder](Data/3_MakingData). If you don't have it already from the last tutorial, you can download the [entire repository](https://github.com/CenterForSpatialResearch/mapping_architecture_urbanism_humanities/) and navigate to the folder, or  with the command line, [download the folder](https://stackoverflow.com/questions/9159894/download-specific-files-from-github-in-command-line-not-clone-the-entire-repo).

This contains:
* nybb - New York City boroughs ([original](https://www1.nyc.gov/assets/planning/download/zip/data-maps/open-data/nybb_21d.zip))
* HYDRO - New York hydrography ([original](https://data.cityofnewyork.us/api/geospatial/drh3-e2fd?method=export&format=Original))
* tl_2019_36_bg - New York State census block groups ([original](https://www.census.gov/cgi-bin/geo/shapefiles/index.php), census block groups for New York state for 2019)

In addition, you **must** download the roadbeds data (it is too big to host on GitHub)
* [Roadbed - New York roadbed](https://data.cityofnewyork.us/api/geospatial/xgwd-7vhd?method=export&format=Original)

### Creating Noise Maps of 311 Data in New York City
#### Downloading 311 Data
The first step in this tutorial is to select, filter and download the 311 data. The [NYC Open Data portal](https://nycopendata.socrata.com/) is a great resource for data related to New York City and it provides an easy way of accessing 311 data. In it's search bar type "311" and it should take you to a list of datasets related to 311 data. The one we are looking for is called **"311 Service Requests from 2010 to Present"**. You may have to select 'View Dataset'

![311](Images/03_00_viewData.png)

Once you've accessed the dataset you will see something like this:

![311 Dataset](https://github.com/juanfrans-courses/mapping_arch_hum/blob/master/Fall_2016/Tutorials/Images/02_Data_Types_and_311/01_311_Dataset.png)
Here, we need to filter the database to download only the records regarding noise complaints for the first 6 months of 2019. You could attempt to download records for a longer period of time, but the files might get too large. To filter the data do the following:
* On the right-hand panel, where it says "Filter", create a small query with the drop-down menus. Where it says `Unique Key`, change it to `Complaint Type`. Keep the `is`, and then type in "Noise" in the space below (The query should read 'Complaint type is Noise'. Make sure there is a check-mark next to the word 'Noise'. You will see how the dataset is filtered and you only get the complaints of type 'Noise'.
* Next, click on `Add a New Filter Condition` and create another query that reads `Created Date` `is between` "01/1/2019 12:00:00 AM" and "07/1/2019 12:00:00 AM".
You should now see the data only for 'Noise' complaints created between the start of 2019 and the end of June 2019.
* Your filters should look something like this:

![311 Filters](Images/03_01_openDataFilters.png)

* Finally, click on the 'Export' button at the top right-hand corner of the site and choose the 'CSV' format. Your file should start downloading then.
* If you open your .csv file in Excel you will see that there are about 32,665 records and that they have both X and Y coordinates and Latitude and Longitude. In the next steps we will use these fields to add the 311 data to a QGIS map.

#### Adding CSV data to QGIS
* First, open QGIS and add the following layers (as vector layers):
  * nybb
  * Roadbed
  * Hydrography
* Organize your layers so that you have the roads on top, then water for New York, then boroughs.
  * Note: if you are having problems with the layers not lining up, not showing, or looking skewed, you may have an issue with the projection. In the lower-right corner of the interface, there is a button which takes you to an interface where you can choose a coordinate system for the project -- try one of the "User Defined Coordinate Systems."
* Now, to add the CSV file we downloaded, click on the `Add Delimited Text Layer` button on the top or left-hand toolbar.

![Add CSV](Images/03_02_addCSV.png)

* In the menu that comes up, look for your .csv (311 data) file. Once you've selected your file QGIS will automatically select some presets. You should have the following options selected:
  * File format: `CSV (comma separated values)` - (this is the format our data is in: each value is separated by a comma)
  * Record options: `First record has field names` - (the first row of our file contains the field names)
  * Geometry definition: `Point coordinates` - (our data has latitude and longitude data)
  * X field: `Longitude` and Y field: `Latitude` - (these are the columns in our dataset that contain our location coordinates)
  * Your menu should look something like this:

![CSV Menu](Images/03_03_csvImport.png)

* Once you click `OK` you might get a warning that says that x number of records were discarded because they didn't have geometry definitions. Click `Close`. There might be some records in the dataset that we downloaded that for some reason didn't include location data.
* Next, QGIS might ask you to select a coordinate reference system (map projection) for this layer. Since we are adding this data based on the latitude and longitude information (decimal degrees, as opposed to feet) we need to select the `WGS 84`, which is the coordinate system that will correctly interpret this data. You will find it under `Geographic Coordinate Systems`. You will find more information on this coordinate system [here](https://en.wikipedia.org/wiki/World_Geodetic_System). Once you select the correct coordinate system, your points will appear on the map.
* *If  you are not prompted, you can double check by double clicking on the layer and navigating to Source, and selecting `EPSG:4326 - WGS 84` under `Coordinate reference system`.*

![layer](Images/03_04_projection.png)

* Even though your points are already on the map, this is just a temporary layer. If you remove the layer, you will need to go through the whole importing process to add them again. To avoid this, we need to export the layer as a shapefile.
* However before you export it, you need to select only the records that have actual coordinate data. If you open the attribute table and look at the `Latitude` or `Longitude` fields you will notice that some entries don't have any geographic data (they are `Null`). We need, therefore, to select only the features that have geographic information and export only those:
  * Open the attribute table and click on the `Select features using an expression` button.

  ![Select Features with Expression](Images/03_05_selectFeatures.png)

  * In this menu we will construct a query selecting only the features that have a `Null` value for latitude or longitude (and then we will invert that selection to get all the records that have geographic attributes and export those as a new layer).
  * The selecting by expression menu has three different panels: the left-hand one is where you will construct your query; the middle one is where you will find the different functions, operator and, more importantly, the attribute table fields; and the right-hand panel will have a description of whatever you select in the middle panel.
  * To build the query, expand the 'Fields and Values' drop-down menu in the middle panel and double-click on 'Latitude'. You will notice the  "Latitude" is added to the left-hand panel. Now type 'IS NULL' after that. This means that we will select only the records where the field 'Latitude' has a `Null` value.

  ![Select Null](Images/03_06_latitudeIsNull.png)

  * Now click on the `Select` button at the bottom right corner.
  * Once you've selected the `Null` records, close the 'Select by expression' window (click the `Close` button). At the top of the attribute table you should read that there are around 544 features selected.
  * Now, switch the selection, so that we only select the records that have correct geographic data. To do this press the `Invert Selection` button at the top:

  ![Invert Selection](Images/03_07_invertSelection.png)

  * Now you should have all the records that have latitude and longitude selected and we can proceed to export them as a shapefile.
  * Close the attribute table, control-click / right-click on the 311 layer and select `Export > Save Selected Features As...`
  * In the menu choose the following:
    * Format: `ESRI Shapefile` - (this is the same format of our other layers)
    * Save as: choose the right location and name your file '311_Data'
    * Make sure you are checking the option that says `Save only selected features`, otherwise you will get an error.
    * Check `Add saved file to map` - (so that once you export the layer, the layer is added to your map)
  * Once you export your layer (and it's automatically added to your map) you can remove the original one by control-click / right-clicking and choosing `Remove`.

#### Symbolizing the Data
The last step in creating a qualitative map of the 311 data is a simple one: we need to symbolize each complaint using its subcategory.
* Right-click on the 311_Data layer and choose `Properties`.
* In the `Symbology` tab, change the drop-down menu that says `Single Symbol` to `Categorized` and then in the `Value` menu select `Descriptor` (this is the field we will symbolize).
* Now click on the `Classify` button at the bottom and you will get all the different sub-categories.
* Lastly, you should change the appearance of the dots by clicking the `Change...` button next to `Symbol`: adjust their size (make them smaller), stroke and fill color (if you wish).
* Once it is to your liking, click 'OK'.
* Finally, change the appearance of the other layers (nybb should be an outline, the water should be blue or removed, the background of the roads should be grey or transparent).
* Create a print composer, add a scale bar, legend, title, source and brief description, and export your map as a PDF or PNG file (Save this to upload it to Canvas)

## Creating a Quantitative Map of 311 Data
Let's say you want to identify which census block group has the highest number of 311 noise complaints. To do this, you first have to join your 311 data to a layer containing the boundaries of New York City's census block groups.
* First, (optionally), add the hydrolin shapefile (hydropol_nyc_)
* Next, add the census block group shapefile (tl_2017_36_bg)
* Move this layer so that it's located below the HYDRO layer.
* If you zoom out, you will notice that this layer includes the census block groups for the whole State. However, we only need the ones for New York City. To select just these block groups we will use the 'select by attributes' method, which means selecting based on data in one of the fields of the layer. The census block group layer contains a field listing the specific county each block group is located in; we will use this field to select only the census block groups located in any of the 5 counties that make up New York City:
  * First, control-click / right-click on the census block group layer and select `Open Attribute Table`. Here you will see the data associated with each of the census block groups. The second column, the one called 'COUNTYFP', contains the county identifiers, and this is the one we will use to select only the New York City block group.
  * Now we need to select all the census block groups that have as their 'COUNTYFP' '005' (Bronx), '061' (Manhattan), '047' (Brooklyn), '081' (Queens) or '085' (Staten Island). To do this click on the `Select features using and expression` button. In this menu we will construct a query selecting only the features that have any of these numbers for their 'COUNTYFP' value.

  ![Attribute Table](Images/03_08_attributeTable.png)

  * To build the query, expand the 'Fields and Values' drop-down menu in the middle panel and double-click on 'COUNTYFP'. You will notice the  "COUNTYFP" is added to the left-hand panel. Now type '=' after that and then click on the `all unique` button below the right-hand panel; this will show a list of all the unique values this field contains. Double-click on '005' to complete the first part of the query on the left-hand panel.
  * The query so far reads "COUNTYFP" = '005'. Notice that the '005' is under single quotation marks. This is because the value is a string (text), not a number. If it was a number you would only type 5, without quotations, and you would be able to do normal math operations with it. Instead, since it's a string, you have to type it with quotations and it behaves like text.
  * Now we need to add the other possibilities, with the operator `or` which we could type or select from the 'Operators' menu. Your final query should read something like this: `"COUNTYFP" = '005' or "COUNTYFP" = '047' or "COUNTYFP" = '061' or "COUNTYFP" = '081' or "COUNTYFP" = '085'`.

  ![Selection Query](Images/03_09_selectCounty.png)

  * Click the 'Select' button on the bottom-right side to select those features that match your query; then close your selection menu and your attribute table. You should see all the census block groups for New York City highlighted in yellow.

  ![Selected Block Groups](Images/03_10_selectedCounties.png)

  * Finally, to create a shapefile with only the selected features, control-click / right-click on the census block group layer and select `Export > Save Features As...`. In the next menu choose the following settings:
    * Format: `ESRI Shapefile`
    * Save as: choose the right location and name your file 'NYC_BlkGrp'
    * Check `Save only selected features` - (this one is very important; if you don't check it you will just export a copy of your original layer with all the features, selected or not)
    * Check `Add saved file to map` - (so that once you export the layer, the layer is added to your map)
  * Click `OK` and you should see a new layer with only New York City block groups.
  * You can remove or hide the New York State blockgroups.
* Now we need to join the 311 data to the census block groups and get a count of how many complaints are in each block group. To do this, click on `Vector` `Analysis Tools` `Count Points in Polygon...`

![Points in Polygon](Images/03_11_countPointsInPolygon.png)

* In the 'Count Points in Polygon' menu choose the following settings:
  * Polygons layer: 'NYC_BlkGrp' - (this is the polygon layer we will join the points to)
  * Points layer: '311_Data' - (this is the layer containing the points that will be joined)
  * Count field name: '311_Count' - (this is a new field that will be created and will contain the count of points that were joined to each block group)
  * Count: Click the ellipsis and select `Save to file`, name it '311_BlkGrp'. Make sure `.shp` is selected as the output file type.

![Counting Points in Polygon](Images/03_12_countPointsModal.png)

* Once you have all your settings ready, click `Run` and let it run. Once it's done, click `Close`. You will see your new layer on the map.
* If you control-click / right-click on the new layer (311_BlkGrp) and choose 'Open Attribute Table' you will see that the last field is called '311_Count' and it contains the number of points joined to each block group. We will use this field to symbolize the block groups.
* To actually symbolize the layer, control-click / right-click on it and choose `Properties`, and in the `Symbology` tab change the 'Single Symbol' drop-down menu to 'Graduated'.
* Next, in the 'Column' drop-down menu select the '311_Count' field to symbolize and click on the `Classify` button to load the values.
* You will notice that QGIS automatically classifies the values into 5 categories. Also, if you look at the settings on the lower left-hand side you will see that the software uses an 'Equal Interval' method for this classification. However, if you click `OK` and look at the resulting map you will notice that most block groups fall within the first group, the one that goes from 0 - 189.2 and that very few are fall in the other ones. In fact, it seems like there is one single block group with more than 900 complaints (located in Staten Island), which is skewing the whole classification method upwards. This is clearly an outlier and the classification method should not be based on this particular block group.
* Instead, go back to the properties and change the classification method to 'Natural Breaks (Jenks)' which deals better with datasets that are not normally distributed. You can find out more about this classification method [here](https://en.wikipedia.org/wiki/Jenks_natural_breaks_optimization).
* You can change your color ramp or the individual colors or strokes of each of the classes. You can also change the number of classes the data is divided into but note that normally, we can only really differentiate between 5 or 6 classes.
* Once you are done with the classification, click `OK` to apply it to the layer and see your results on the map.

![Classification Methods](Images/03_13_jenks.png)

Lastly, we need to hide the census block groups that fall outside of the New York City borough boundaries. If you look closely at the census block group layer, you will see that there are some block groups that fall inside the Hudson River and that shouldn't be included in our map.

There are a couple of ways of doing this: one option would be to clip the block group layer using the borough layer, in order to get rid of the census block groups that fall outside the boroughs. However, this option would permanently modify the block group layer and, if at any point the borough boundaries don't align perfectly with the block groups (which is entirely possible), the geometry of those block groups would be changed too. The best option then is to hide the block groups that fall inside the water and conveniently enough there is a field in the block group attribute table that has a specific value for these features.
* First, open the attribute table of the 311_BlkGroup layer. You will notice that there is a field called 'ALAND' and another called 'AWATER'. 'ALAND' one has a unique identifier for each of the block groups that has some land area; 'AWATER' has an identifier for those block groups that have some water. There problem is that some block groups have both water and land. So we will only show those block groups where the 'ALAND' field does not equal 0, meaning that they have some land.
* To do this we will create a 'Feature subset'. Open the layer properties and go to the `Source` tab. At the bottom of this tab you will see the 'Provider Feature Filter' panel. Go to the bottom of this panel and click on the `Query Builder` button. This query builder will work in a similar way as the 'Selection by attributes' query builder.

![querybuilder](Images/03_14_queryBuilder.png)

* In the 'Fields' panel you will see the 'ALAND' field. Double-click on this to make it appear in the bottom panel ('Provider specific filter expression').
* Now add '!= 0' to the expression. ('!=' means 'does not equal').
* Your expression should look something like this:

![Query Builder](Images/03_15_buildQuery.png)

* Click `OK` in the 'Query Builder' and then `OK` again in the 'Properties' panel. Your map should now only show the census block groups that have land.

You may wish to change the projection of the whole project at this point. It should be in NAD83/New York Long Island EPSG 2263. Click on the project projection in the bottom right hand corner to open the project projection options.

![Invert Selection](Images/03_16_changeProjection.png)

![Invert Selection](Images/03_17_correctProjection.png)

At this point, you can adjust colors, strokes and layer order. The water-related layers should be light grey with transparent boundaries. The classified 311 block group should appear to lay on top, with the boundaries of the boroughs visible on top. You can, of course, style it as you wish, however.

And finally, create a print composer, add a legend, title, explanation, source and a scale bar, and export your map as a PDF file. Your final map should look something like this:

![Final Map](https://github.com/juanfrans-courses/mapping_arch_hum/blob/master/Fall_2016/Tutorials/Images/02_Data_Types_and_311/11_Final_Map.png)

#### Deliverables
Upload your two (PDF) 311 data maps to canvas. Your map should include proper legends, scale bars, titles, explanations and sources.


______________________________________________________________________________________________________________

Tutorial created by Juan Francisco Saldarriaga (jfs2118@columbia.edu) for the Mapping for Architecture, Urbanism and the Humanities class at Columbia University. Edited by Michelle McSweeney for Fall 2017, Brian House for Fall 2018, and Emily Fuhrman for Spring 2021.
