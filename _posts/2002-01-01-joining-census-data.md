---
title: "Census Exercise"
layout: post
category : Clean your Data
tagline: "Supporting tagline"
tags : [applications, qgis, quantumgis]
---

{% include JB/setup %}

## What this exercise covers:

 - Classify data field types for CSV
 - Import CSV into QGIS
 - Import vector layer
 - Join CSV to vector layer
 - Save vector layer with joined CSV
 - Field calculator
 - Symbolize vector

## Data

Data for this exercise is located at iPlant Datastore: Community Data/aegis/Spatial-bootcamp/Exercise_2/

- home-values.zip
- housing-units.zip
- tl_2014_53_tracts.zip

## Procedures

1. Download Exercise_2 data and unpack each zip file:<br>
![Spatial Data Carpentry: Unpack zip files]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-unzipped.png)
1. Import Census Tracts shapefile **tl_2014_53_tract.shp** ![Spatial Data Carpentry: Census Join - Add vector layer]({{BASE_PATH}}{{ASSET_PATH}}/images/qgis/add-vector.png).<br>
![Spatial Data Carpentry: Census Join - Washington Census tracts]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-1.png)
1. Rename tl_2014_53_tract to census-tracts:<br><br>*Right-click layer > Rename > census-tracts*<br><br>
1. Open the census-tracts **Attribute table** and locate a column that could possibly be unique to each Census tract used for joining (HINT: it's GEOID):<br><br>*Right-click census-tracts (layer list) > Attribute table*<br><br>Close the Attribute table once you've located the primary key.<br><br>
![Spatial Data Carpentry: Census Join - Washington tract attributes]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-2.png)
1. Navigate to your Exercise_2 directory and open housing-units.csv and inspect the data for columns to use for our joins (primary key). HINT: it's GEO.id2.<br><br>Reminder: We can only join data (tabular join) with *exact same* columns. See the tables below:<br>
![Spatial Data Carpentry: Census Join - Locate similar columns]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-5.png)
1. Clean the data. For the purposes of this exercise, we do not need the removed data. Our objective is to join data and visualize (with minimal data wrangling). In LibreOffice, highlight the row/column, right-click and Delete Selected Columns. Excel should be similar.<br><br>
For **housing-units.csv**:
- Remove the second row containing the annotations (subheader). 
- Delete the columns: GEO.id, GEO.display-lable, and HD02_VD01.
- Your file should look like the image below:<br>
![Spatial Data Carpentry: Census Join - Delete housing data annotation]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-6.png)
1. Rename header HD01_VD01 to TotalUnitsHome. Save your document, be sure it's stored as a Text CSV ![Spatial Data Carpentry: Census Join - Save as Text CSV]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-8.png). Excel users may have to select Text CSV from a drop-down list. Renaming the headers reduces confusion down the road. Also, notice how there are no spaces in the header name. Removing (or not having) spaces is in the top 10 list of best naming convention practices (on all computer systems.. everywhere in the digital world.. you should never have spaces in your file names or directories). Close the document.<br>
![Spatial Data Carpentry: Census Join - Rename HD01_VD01 to MeanValueHome]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-7.png)
1. Do the same for **home-values.csv**:<br><br>
* Navigate to document
* Delete second row with annotations (subheader)
* Delete columns: GEO.id, GEO.display-label, HD02_VD01
* Rename HD01_VD01 to MedianValueHome
* Save your document in Text CSV format ![Spatial Data Carpentry: Census Join - Save as Text CSV]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-8.png)<br>
![Spatial Data Carpentry: Census Join - Wrangle Home Values CSV]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-9.png)
1. You're not quite finished with wrangling your data yet. If you have a quick look at your home-values.csv, you'll notice there are some invalid fields - number fields that contain non-numeric values. Lines 501 and 502 read 1,000,000+ and should be changed to 1000000.<br>
![Spatial Data Carpentry: Census Join - Wrangle Home Values CSV]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-10.png)
Aslo, there are some median home value records with '-' and if you read your metadata you would know that:<br><br>*An '-' entry in the estimate column indicates that either no sample observations or too few sample observations were available to compute an estimate, or ratio of medians cannot be calculated because of one or both of the median estimates falls in the lowest interval or upper interval of an open-ended distribution.*<br><br>Let's cleanse this now:<br> LibreOffice and Excel users: **Find and Replace '-' (without single quotes) with NULL**<br><br>Also, after making these changes the data type of the incorrect records have not changed - they're still string/text fields in a numeric universe (only in LibreOffice/Excel, however). We will instruct QGIS on how to read the column data types with a CSVT. Be sure to save your home-values.csv in Text CSV format ![Spatial Data Carpentry: Census Join - Save as Text CSV]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-8.png).
![Spatial Data Carpentry: Census Join - Wrangle Home Values CSV]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-11.png)
1. Just wait right there, one does not simply join a string (tract's GEOID) to a number (housing data's GEO.id2).<br><br>
We now have to create our CSVTs (one for each home-values.csv and housing-units.csv). Think of a CSVT as a 'comma-separated value data type' file. As a reminder, these indicate to QGIS the data types of each column of repsective CSVs.<br><br>
Our goal here is to join GEOID from tracts (shapefile) to GEO.id2 from home-values.csv and housing-units.csv.<br><br>
Go back into QGIS and open the census-tracts properties and navigate to **Fields**. Locate GEOID in the table and notice its **Type name** - **String** (see image below). With less wrangling we can make GEO.id2 a string for joining purposes. Close the properties window.<br><br>
The following steps outline how to create the CSVTs.<br>
![Spatial Data Carpentry: Census Join - Tract field data types]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-12.png)
1. Open a text editr (gedit, VIM, notepad, notepad++) and enter the following:<br>
``"String","Real(12.0)"``<br>
![Spatial Data Carpentry: Census Join - Creating a CSVT]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-13.png)
What this means:<br>
There are two columns in this file separated by a common. The first column indicates string and the second is a real number with length of 12 and precision of 0. While preparing this exercise, QGIS didn't like multiplying integers (Field Calculator results with NULL), so we're using real number type. 0 precision removes unneccessary zeros after the decimal from real number types, also there are no fractions in the datasets.<br><br>
* Save this file as *home-values.csvt* within the *same* directory as *home-values.csv*<br><br>
1. Luckily, we wrangled both CSVs into the same format, so copy *home-values.csvt* and paste it into the *same* directory as *housing-units.csv*. Rename this pasted file as *housing-units.csvt*. Data types, lengths, and precisions will be the same for both files.<br>
![Spatial Data Carpentry: Census Join - Viewing housing-units.csvt]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-14.png)
1. Import CSVs into QGIS.<br><br>
Don't move just yet, there a little quirk about importing CSVs with CSVTs into QGIS. You *cannot* use the **Add Delimited Text Layer** tool, this does not recognize the CSVT precision and length.<br><br>
The best way to import with CSVTs is the drag-and-drop method.<br><br>
Drag-and-drop your housing-units.csv and home-values.csv into QGIS. Don't touch the associated CSVTs, QGIS reads these during the import process, just *don't* move these files. Just as we keep all shapefile files in one directory.<br>
![Spatial Data Carpentry: Census Join - Drag and drop CSVs]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-15.png)
1. Your workspace should look like the image below. Save your workspace now.<br>
![Spatial Data Carpentry: Census Join - Workspace]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-16.png)
1. Now we are able to join our Census tabular data to the tracts shapefile. Join data by opening the **Properties** of the census-tracts layer and navigating to the **Joins** tab.<br>
![Spatial Data Carpentry: Census Join - view tracts properties]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-17.png)
1. Start the first join by clicking the green plus sign ![Spatial Data Carpentry: Census Join - green plus sign]({{BASE_PATH}}{{ASSET_PATH}}/images/qgis/join-plus.png). We're joining home-values and housing-units to census-tracts with:<br>**Join field: GEO.id2<br>Target field: GEOID<br>Cache join layer in virtual memory: (checked)<br>Apply and OK onced both joined**<br>
![Spatial Data Carpentry: Census Join - joining data]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-18.png)
1. Open census-tracts **Attribute Table** and locate the MeanValueHome and TotalUnitsHome fields. Notice they've been renamed and cut-down. We can still decrypt these headings so we'll continue on. Close the attributes table.<br>
![Spatial Data Carpentry: Census Join - viewing joined data]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-19.png)
1. You have joined the data but it's currently being stored in the virtual memory, so we need to save census-tracts (with joined data) as a new shapefile:<br><br>*Right-click shapefile > Save As*<br>
**Format: ESRI Shapefile<br>
Save as: tracts-values-units (save in your project directory)<br>
Add saved file to map: (checked)**<br>
![Spatial Data Carpentry: Census Join - save shapefile]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-20.png)
1. We need to convert *home-value* to thousands of dollars to prevent <a href="http://en.wikipedia.org/wiki/Integer_overflow" target="_blank">integer overflow</a>:
- **Open tracts-value-units attributes table > Toggle editing mode** ![Spatial Data Carpentry: Census Join - toggle edits]({{BASE_PATH}}{{ASSET_PATH}}/images/qgis/toggle-editing-mode.png) **> Field Calculator** ![Spatial Data Carpentry: Census Join - field calculator]({{BASE_PATH}}{{ASSET_PATH}}/images/qgis/field-calculator.png)<br>
- Calculate a new field:<br>Create new field: **(checked)**<br>Output field name: **HmValThnds**<br>Output field type: **Whole number (integer)**<br>Output field width: **10**<br>Expression: **"home-value" / 1000**<br><br>
![Spatial Data Carpentry]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-hmvalthnds.png)
1. Let's now calculate total housing value per census tract (median home value X total housing units). 
* Configure Field Calculator inputs as follows:<br>
**Create a new field: (checked)**<br>
**Output field name: TotalValue**<br>
**Output field type: Decimal number (real)**<br>
**Output field width: 20**<br>
**Precision: 0**<br>
**Expression: "HmValThnds" * "housing-un"**<br><br>
![Spatial Data Carpentry: Census Join - field calculator]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-totalvalue.png)
1. Locate **TotalValue** and confirm the calculation - total home value per Census tract in thoudsands of dollars.<br><br> NOTE: you should still have NULL values given that home-values.csv contains NULL values.<br><br> Be sure to *Save edits* ![Spatial Data Carpentry: Census Join - save edits]({{BASE_PATH}}{{ASSET_PATH}}/images/qgis/save-edits.png) and disable editing mode (Toggle editing mode) ![Spatial Data Carpentry: Census Join - toggle editing mode]({{BASE_PATH}}{{ASSET_PATH}}/images/qgis/toggle-editing-mode.png). The editing options should now be grayed-out and disabled.<br>
![Spatial Data Carpentry: Census Join - TotalValue per tract]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-22.png)
1. You've successfully joined two sets of Census tabular data to a shapefile.<br><br> 
1. Symbolize tracts-values-units by categorizing by TotalValue and select a color ramp.<br><br>Open style properties: *Right-click tract-values-units (layers list) > Properties > Style*<br><br>Style by: *Categorized*<br><br>Column: *TotalValue*<br>Color ramp: *(select ramp)*<br>Classify - "Classification would yield 1422 entries which might not be expected. Continue?" *Yes*<br>
QGIS will inform you that you have a high number of classes, click OK to confirm the categorization.<br><br>
We'll need to remove NULL values. Locate NULL within the category table (HINT: it contains no values and is blank - look towards the bottom), highlight and click 'Delete' (not Delete all!) - 'Delete' removes only selected rows.<br><br>
Click Apply then OK to confirm changes.<br><br>
![Spatial Data Carpentry: Symbolize census tracts]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-symbolize.png)
1. Deactivate census-tracts layer by unchecking its activate box in the layers list.<br><br>
Your your results should resemble the image below<br><br>
Save your workspace. <br>
![Spatial Data Carpentry: Census Join - completed]({{BASE_PATH}}{{ASSET_PATH}}/images/census-join/census-join-24.png)

<br>

----
