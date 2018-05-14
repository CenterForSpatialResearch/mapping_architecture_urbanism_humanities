## Making Data & Data Types

### Geocoding coordinates and street addresses

#### Premise

In this exercise you are going to map locations related to music in the Bronx by geocoding – converting tabular data into spatial mapping files. First, you will take geographic coordinates and display them visually. Then you will take a list of street addresses and locate them by using a geolocation service, matching up the addresses against a database of locations.

By the end of this tutorial, you will be able to:
* Find data with a spatial component
* Georeference that data to use in QGIS
* Analyze that data to answer a question about music production in NYC

#### Notes on the data: 

You will use a table of business locations  from the ReferenceUSA business directory. There is a sample dataset from ReferenceUSA in the [Data Folder](https://github.com/michellejm/mapping_arch_urban_hums/tree/master/Data/2_MakingData).  You will locate the businessses with the geocoding services from OpenStreetMap and Google. 

### Geocoding Exercise

First, you will find businesses in the music industry located throughout the Bronx.  You are going to use the ReferenceUSA business directory to identify these businesses.  ReferenceUSA is a comprehensive database of business locations throughout the US.  Columbia Libraries maintains a subscription to this service. 

Go to the CU Libraries’ homepage and search for “ReferenceUSA” in the quicksearch bar:
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode1.png)

The first result should provide a link to ReferenceUSA:
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode2.png)

Open the database and click on the “U.S. Businesses/ Employers” database: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode3.png)

Click on the advanced search tab.  Here, you have a variety of tools to limit the business search.  First you will limit by geography.  Check “county” in the geography list and select “New York.”  Add “Bronx, NY” to the search selections: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode4.png)

Next, you will search by business type.  Choose the “Keyword/SIC/NACIS” option under business type. Select “search all NACIS” (a business classification system created by the census department).  All of the businesses in the directory will have at least one of these classifications.  You can search for classification entries by keyword and add them to your search limits.  Below I have added all of the NACIS classifications including the terms “Musical,” “Music,” and “Sound.”  When I click on “Update Count” on the right it finds 129 entries:
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode5.png)

You may opt to use a different list of classifications, but if so, try not to use a search that finds more than 200 results because ReferenceUSA only allows about 200 to be downloaded at once.  Once finished click on “View Results”:
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode6.png)

The results screen only shows 25 results per page.  To download all results, you will have to check each entry. You can select the whole page by using the topmost selection box by “company name,” but you will have to toggle thorough all 6 pages to do them all:
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode7.png)

Once all the businesses have been selected, click on “download.”  Select 'comma delimited' as the file format.  Under level of detail select “custom,” ReferenceUSA provides a wealth of detail for each business-too much for our purposes, so you will select the attributes manually. You should be given the company name and address information by default, be sure to add the “latitude” and “longitude” fields: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode8.png)

Feel free to add any more attributes that interest you.  Once finished, download the file.  Save it as MusicBusinessesBronx.csv in your project directory.  If you open it, you can see that the results should be very clean, with delimited address fields and coordinates in decimal degrees: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode9.png)

Next, open QGIS: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode10.png)

You are going to use OpenStreetMap as our reference data for the geocoding process. You can access the OpenStreetMap service through the MMQGIS plugin.  This plugin does not come pre-installed with QGIS, so you need to install it.  Under the plugins menu, select  “Manage and Install Plugins…” 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode11.png)

Search for “MMQGIS”, and click “Install plugin”: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode12.png)

It may take a few seconds to install.  

You will also install the OpenLayers plugin. OpenLayers will allow you to view and navigate through several different basemap services including OpenStreetMap, Google Maps, Bing Maps, and MapQuest. Find the OpenLayers plugin and install it in the same fashion:
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode13.png)

Close the plugins menu when finished. 

The OpenLayers tools should now appear under the Web menu.  Expand it and open the OpenStreetMap layer:
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode14.png)

Use the zoom tools ![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode15.png)to zoom in to the Bronx area: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode16.png)

Now you are going to map the Bronx business locations you downloaded from ReferenceUSA.  First you will use the latitude and longitude coordinates that Reference USA provides. 

In QGIS, add the table you downloaded by clicking on the “Add Delimited Text” tool ![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode17.png). Navigate to your table.  Choose csv as file format, point coordinates as your geometry, and “longitude” and “latitude” as your X and Y fields (these may all be selected by default, as QGIS tries to guess the right entries, but make sure that the settings are correct).  Click OK: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode18.png)

Choose WGS 84 as the coordinate system: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode19.png)

Your locations should map on top of the Bronx: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode20.png)

The points that you have displayed are being stored temporarily “on the fly” in computer memory.  If you are happy with the results, it is a good idea to save the data immediately in a spatial format.  An easy way to do this is to right-click on the MusicBusinessesBronx layer in the layers panel and clicking “Save as..”: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode21.png)

Save the file as an ESRI Shapefile in your project directory with the name “MusicBusinessesXY”: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode22.png)

The new layer should add to the map over the first.  Now you can turn off the first Business layer by unchecking it in the layers panel: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode23.png)

Exactly how ReferenceUSA derives the coordinates for these businesses is something of a mystery; it isn’t described in the website or documentation.  Also, in other circumstances you won’t have coordinates for your locations.  Next, you will geocode the business locations using a street address locator.

The QGIS address location tools are in the MMQGIS plugin you installed earlier. The MMQGIS tools should now appear under a new MMQGIS menu: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode24.png)

MMQGIS provides a disparate set of tools, but you will only be using the ‘geocode’ toolset.  The geocode toolset presently includes three functions. “Geocode from street layer” allows you to build your own geocoding service based off of an addressable street layer database.  "Street Address Join" allows you to locate a single-field location against a gazatteer table. “Geocode CSV with Google/ Open Street Map” allows you to geocode a list of locations against the geocoding services available through either Google or OpenStreetMap (OSM).   For this exercise you will be use the OSM geocoder.  The OSM geocoding service, like all of the OSM data, is open source and transparent.  We know, for example, that OSM uses the LION street locations from the NYC Planning Department for its New York address locator, so you can expect very good results.

Since ReferenceUSA provides address locations in a very clean, appropriately delimited format, this will be an easy process for you that won’t require any data cleaning.  Choose “geocode CSV with Google / OpenStreetMap” from the MMQGIS menu: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode25.png)

Navigate to your business table and input “Address” as the address field, “City” as the city field, and “State” as the state field.  There is not an appropriate country field here, so leave that blank.   Choose OpenStreetMap as your web service.  Save the Output as “MusicBusinessesOSM” and save an output list as “NotFoundOSM” in your project directory. The not found output will create a table of any locations that cannot be located (there may not be any for this exercise). Click OK:
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode26.png)

This may take a couple of minutes to run.  When it is finished the new layer should be added to the map (you may want to change the color or size of the locations in the layer properties if the two layers look too similar or small): 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode27.png)

It is likely that the two business location layers will be significantly different.  In the example above, most of the locations seem to be offset by a few feet, but in other cases by quite a bit more.  

In some cases, it is not clear if there what the corresponding location is between the two layers: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode28.png)

You can measure how much discrepancy there is using QGIS tools. One option is to look for the nearest neighbor for each feature.  We can test this quickly by using the built in Distance Matrix tool:
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode29.png)

Distance Matrix will take the points in one layer and find the closest point in another layer and return the distance between the two.  Open the tool and use MusicBusinessXY as the input and MusicBusinessesOSM as the target layer. Use the company name as the ID field and be sure to check “use only nearest target points.”  Save the output as DistanceMatrix. Click OK:
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode30.png)

Navigate to the output table and open it: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode31.png)

Note that the distance measurements are given in decimal degrees (because of the polar coordinate system that the data is currently on) so the distances themselves are not particularly meaningful.  What is important is to note that the distances vary quite a bit.  Furthermore, in many cases the nearest neighbor is not the same business!  In some cases this is because the locations in the area are significantly different.  In other cases it is because OSM was unable to find the business at all. 

Open the NotFoundOSM table in your project directory.  In my example there were 31 addresses that were not found: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode32.png)

This particular geocoding toolset does not give the user much control over the way in which the tools work.  Nor is there any indication of how “close” the locator was to finding a location or what the reason might be for not identifying a location.  Possibilities include bad zip codes, misspelled or alternative street names, confusing apartment numbers or post office boxes.  The OSM service seems to be somewhat conservative in its estimation of these locations.  If you like, you can investigate this table, edit it and geocode it again to see if the OSM geocoder might find the businesses.

Try the Google geocoder and see if it finds more results.  The Google geocoder includes a number of additional location services, although it is all proprietary and obscure – a “black box.”  Also, it is a proprietary service and Google only allows a small number of searches free of charge (approximately 2,000 or so a day) although this should not be a problem with your small dataset.  Run the geocoding tools as above only this time choose the Google Maps web service.  Save the output files as MusicBusinessesGoogle.shp and NotFoundGoogle.csv: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode33.png)

Click OK, it may run for a couple of minutes.  The new layer should add to the map automatically: 
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode34.png)

In most cases, the different locators will find very similar but different locations.  For these two businesses on Tremont Ave. the three locations are all approximately the same building:
![GeocodingExercise](https://github.com/CenterForSpatialResearch/MappingForTheUrbanHumanities/blob/master/Tutorials/Images/MakingData02/Geocode35.png)

In other cases the discrepancies are likely to be much larger.  Spend a few minutes comparing the three layers and how and where they differ.  

* Which service has fewer unfound results?
* What are some advantages/disadvantages to a liberal coder versus a conservative one?

Looking at the attribute table in the Google results, there is an “addrolocat” field which gives some indication of how Google identified the location.  In most cases this is indicated as “street_address” but there are other entries like “premise” and “subpremise.”

Now add the [NYC Neighborhood Tabulation Areas](https://drive.google.com/drive/folders/0B8IdSWLrkSd3cVlORklvUkVIQ2s?usp=sharing) boundary to the project. 

Join the music locations layer to the nynta layer based on location 
* Go to Vector ‣ Data Mangement Tools ‣ Join attributes by location
* The Target Vector Layer is the one we want to add to (nynta), the Join Vector layer is the music establishments.

Create a chloropleth map of music locations by 


Answer the following questions:
1. How many music related locations are in the Bronx?
2. Which neighborhood has the most locations?

### Deliverables: 

** due Thursday, September 21, 2017**
For Thursday, email your finished map composition (as a pdf or jpeg) and the answers to your questions to Michelle at mam2518@columbia.edu

______________________________________________________________________________________________________________

Tutorial adapted by Michelle McSweeney from a tutorial written by Eric Glass, for *Mapping for the Urban Humanities*, a intensive workshop for Columbia University faculty taught in Summer 2017 by the [Center for Spatial Research](http://c4sr.columbia.edu). More information about the course is available [here](http://c4sr.columbia.edu/courses/mapping-urban-humanities-summer-bootcamp).

