# Tutorial 10: Tableau

In this tutorial, we will leverage the personal spatial dataset you have been constructing up until this week to create a ["small multiples"](https://en.wikipedia.org/wiki/Small_multiple) visualization in Tableau. The small multiples technique is great for showing subtle differences in the same region across different dimensions, including time. Outlets like the [NYT](https://www.nytimes.com/interactive/2014/09/08/us/climate-change-could-push-birds-north-shrink-their-ranges.html?_r=0) and [FT](http://www.ft.com/cms/s/0/e789b666-4b44-11e5-b558-8a9722977189.html) have put this technique to good use. (For more inspiration, you can also check out the gallery [here](https://www.pinterest.com/emilyfuhrman__/small-multiples-maps/)). During a period in which many of us are staying at home, what better way to represent the subtle, day-to-day differences that follow the passage of time?

To start, download and install [Tableau Public](https://public.tableau.com/s/download), the free desktop version of Tableau. Create a Tableau Public account while you're at it, because you will need one to save and/or publish your work.

## Preparing our data

Each personal spatial dataset is different. Yours might be image-based or hand-drawn; quantitative or qualitative. For my example dataset, I have a collection of ten photos I took of the sky over Transmitter Park in Greenpoint across ten different, consecutive Mondays: 

![Sky Dataset]

If you took multimedia records of any kind, including photos or drawings, now is the time to import them into your machine.

No matter _what_ you decided to track and record since the start of the semester, your dataset should at least include metadata around the date you added your record, and the location of that record. (My location is basically the same for each photo, though the time of day does differ.)

As such, to prepare your data for import into Tableau, open a new Google Sheet or Excel file, and start by setting up the following columns:

* `date`
* `longitude`
* `latitude`

![Dataset Setup]

You should have at least ten entries, one for every week of this class so far. I've recorded each of my entries as a row in my table. If you did not record your lat/lon, a quick lookup on Google Maps can enable you to grab each value from the URL; see below.

![Google Maps]

Add a column next to your `date`, `longitude`, and `latitude` columns. Here is where you will enter the values for the main dimension you tracked. If you took periodic temperature readings for your dataset, you might call this column `temp`, and enter each day's temperature in degrees; if you recorded your mood for each day, you might call this column `mood`, and enter the descriptors you recorded for each day. If you are working with multimedia files (ex. photos or scanned images), you can use this column to record the filenames of each image. 

![Main Record]

Now, add as many additional columns as you like to quantify, standardize, or otherwise add more metadata to your records. Think about what visual variables you might want to have at your disposal when visualizing this data. Some ideas:

* Define your own "scale" to draw out quantitative dimensions of qualitative data. For instance, if you recorded your mood, create a column called `happiness_scale` to translate your terms onto a 1-10 range.
* If working with images, summarize an aspect of those images. Try recording a color sample of each image, or measuring an angle of a feature in each image.
* Define a "category" to draw our qualitative dimensions of quantitative data. For example, if you took numeric temperature recordings, add an additional column called `temperature_category` and classify the readings from a "Hot" -> "Warm" -> "Cool" -> "Cold" scale.
* Add additional column(s) to capture features that only appear in some of your records. 

For my dataset, I recorded the hex code of a color sample I took from each photo using Photoshop. I also added columns for a custom "cloudiness" scale (1 = least cloudy, 5 = super cloudy) to capture how much cloudage is visible in the frame, as well as an additional boolean `birds_visible` column that records whether birds are visible in the frame. I also added a `time` column to record the time of day each photo was taken. While you may not end up using every column, it is always better to have a rich dataset to work with. See my additional columns highlighted below:

![Custom Columns]

Export your finished table as a CSV, and name it `spatial_dataset.csv`. Now, we're ready to import this data into Tableau. 

## Importing our data

Open up Tableau Public. In the left column of the welcome screen, you will see several different ways to import data into the application. Click `Text file` and select your CSV file from the dialog that pops up.

![Tableau Import]

Once you have imported your data, you will see a preview of the columns you defined. Make sure that Tableau has detected the data type in each column correctly: any `String` columns should have an `Abc` next to the name, any numerical columns should have a `#` next to the name, and geodata should have a globe icon next to its name. 

![Tableau Preview]

If everything looks good, click the orange-highlighted `Sheet 1` tab in the bottom left corner of the screen. (It should have a `Go to Worksheet` tooltip hovering over it.) Now, you should see a blank worksheet, with your familiar variables on the left and a blank space in the middle.

![Tableau Worksheet]

You will notice that your variables have been sorted roughly into qualitative (blue) and quantitative (green) categories. This categorization is driven by the data types you confirmed in the earlier preview of your imported dataset.

Tableau's interface uses worksheets like this one in a similar way to Excel. Each worksheet is a space within which to construct a specific visualization. Once you have constructed any number of visualizations, you can pull them into more complex display types, like Dashboards and Stories (you can see these additional tab options below). 

## Setting up the multiples grid

There are a few different ways to create a small multiples display in Tableau. We could create a bunch of individual maps in several different Worksheets and pull them into a single Dashboard, or we could create a grid of maps in a single Worksheet. In this tutorial, we will focus on the second method.

From the left pane, drag your `Longitude` field to the `Columns` bar at the top, and your `Latitude` field to the `Rows` bar. The display should update to the region your dataset spans. In my case, that is a single point on the edge of Brooklyn.

![Lon/Lat Map]

Now, because I have ten different entries in my dataset, I will aim to create a 5x2 grid, starting with the earliest Monday in the top left and the most recent Monday in the bottom right. This means I will have to create some additional variables to tell Tableau which column or row I want each entry to appear in. Right-click anywhere in the left panel and select `Create Calculated Field...`

![Calculated Field]

* Name the first field `Row Number`. Click the right carat to extend the dialog to include a query builder.
* Add `CASE` followed by the name of the variable you want to anchor on. In my case, since I want to order my maps by date, I will enter `[Date]` (this will also autocomplete based on my available dimensions).
* We will be adding a specific parameter for _every entry_ to assign it a row. Since I want to order my maps by date, I will have to go date by date. So, on the next row, I will type: `WHEN DATE("2020-11-09") THEN 1`. The value `2020-11-09` is the first date in my dataset. I am forcing it through a `DATE()` function to make sure Tableau detects it properly. The `1` refers to the row number I am assigning it.

![Row Number]

* Add an additional row for every entry in your dataset. Assign each value its row number. (I switch to assigning entries to row #2 after the first five.)
* When you have entered row numbers for all of your entries, type `END` at the end.

![Row Numbers]

* Click `OK` to add `Row Number` as a new calculated field.

Now, we will do the same for column numbers. I need to think again about the order in which I want my maps to appear. Because I want two rows of five maps each, I will need to assign consecutive columns to each entry.

* Create a new calculated field
* Name the field `Column Number`
* Enter the same query again, but this time make sure the numbers reflect the columns the entries will need to go in. In the below image, note that I count _up_ from 1 to 5, and then start counting up _again_ after the first five.

![Column Numbers]

* Click `OK` to add `Column Number` as a new calculated field.

Now we are ready to turn the current map display into a grid of smaller maps.

* Drag the `Column Number` up to the `Columns` bar
* Drag the `Row Number` up to the `Rows` bar

![First Drag]

But wait.. this doesn't look right. We need to do a couple more things. For both the `Column Number` and `Row Number` fields, click the small right arrow on each and make sure to select `Dimension` instead of `Measure`.

![Dimension to Measure]

Next, click the same arrows for each field and select `Discrete` instead of `Continuous`. 

![Discrete to Continuous]

That's more like it!

## Symbolizing our multiples field

At this point, you can play around with the custom columns you added to your dataset to symbolize different dimensions in different ways. Play with the `Marks` palette to tweak size, color, and symbol type. Try one of the following:

* Drag a categorical dimension to the `Color` tile color your points by category
* Drag a continuous dimension to the `Size` tile to size your points based on a value

### Focus: Adding custom images

If you worked with individual images in your dataset, you might at this point want to bring them into your display.

* Search for the folder `My Tableau Repository` on your machine
* Find the `Shapes` folder within that folder
* Create a new folder with your chosen name, and add your images to that folder

![Tableau Shapes]

Open up Tableau again. 

* In the `Marks` menu, select the `Shape` option

![Shape Option]

* Drag a dimension with unique values (such as `Date`) to the `Shape` tile that appears
* Click the `Shape` tile
* Select `More Shapes...`
* In the dialog, click `Reload Shapes` to make sure Tableau knows you have added your own images
* Find and select your custom folder in the dropdown

![Custom Shapes]

* Click `Assign Palette`, and re-match individual images to each entry if needed.
* To adjust image size, click the `Size` tile and drag the slider.

![Image Size]

### Focus: Styling map

Tableau offers a similar way to control the look and feel of your basemap as QGIS and Mapbox do. You can use a layers view to adjust styling and visibility.

* In the top menu, select `Map > Map Layers...` to bring up a layers view, like we have seen in both QGIS and Mapbox.
* Here, you can toggle layer visibility on or off, select a different style for your basemap, or adjust the opacity of different layers.

![Layers Panel]

### Focus: Adding labels

It is important to label each cell of the grid to indicate what it represents. Here, I will label each map in my grid based on the date I recorded each entry.

* Drag the `Date` field to the `Label` tile in the `Marks` panel
* Click the triangle within the `Date` field to select your desired format
* Click the `Label` tile to edit font and color of each label

![Date Format]

### Focus: Axis cleanup

Leaving in the `Column Number` and `Row Number` labels looks a little messy; let's remove them.

* Right click `Row Number` and select `Hide Field Labels for Rows`
* Right click `Column Number` and select `Hide Field Labels for Columns`
* For the individual number labels on both x- and y-axes of the grid, right click and _uncheck_ `Show Header`

![Unshow Header]

Now, add a title to your map. Hide any legends that you do not need. Perhaps your final artifact is poetically mundane, like the following?

![Final Map]

## Deliverables

To save your work, sign in with the Tableau Public account you created at the beginning of the tutorial. You may publish your map to your account if you'd like, and include the link with your submission. You can also download an editable `.twbx` file of your project from the Tableau Public site.

On Canvas, submit your finalized spatial dataset (including custom columns) as a CSV file along with a screenshot of your Tableau small multiples map. 

______________________________________________________________________________________________________________


Tutorial written by Emily Fuhrman for Mapping for Architecture, Urbanism, and the Humanities Spring 2021.




[Sky Dataset]: Images/10_00_sampleData.png
[Dataset Setup]: Images/10_00_datasetSetup.png
[Google Maps]: Images/10_01_googleMapsLatLon.png
[Main Record]: Images/10_02_mainRecord.png
[Custom Columns]: Images/10_03_customColumns.png
[Tableau Import]: Images/10_04_tableauImport.png
[Tableau Preview]: Images/10_05_tableauPreview.png
[Tableau Worksheet]: Images/10_06_tableauWorksheet.png
[Lon/Lat Map]: Images/10_07_lonLatMap.png
[Calculated Field]: Images/10_08_calcField.png
[Row Number]: Images/10_09_rowNumber.png
[Row Numbers]: Images/10_10_rowNumbers.png
[Column Numbers]: Images/10_11_columnNumbers.png
[First Drag]: Images/10_12_firstDrag.png
[Dimension to Measure]: Images/10_13_dimensionMeasure.png
[Discrete to Continuous]: Images/10_14_discreteContinuous.png
[Map Grid]: Images/10_15_mapGrid.png
[Tableau Shapes]: Images/10_16_tableauShapes.png
[Shape Option]: Images/10_17_shapeOption.png
[Custom Shapes]: Images/10_18_customShapes.png
[Image Size]: Images/10_19_imageSize.png
[Map Layers]: Images/10_20_mapLayers.png
[Layers Panel]: Images/10_21_layersPanel.png
[Date Format]: Images/10_22_dateFormat.png
[Unshow Header]: Images/10_23_unshowHeader.png
[Final Map]: Images/10_24_finalMap.png
