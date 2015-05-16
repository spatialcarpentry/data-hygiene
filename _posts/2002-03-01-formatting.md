---
title: "Data Formatting"
layout: post
category : Clean your Data
tagline: 
tags : [wrangling, formatting]
---

#### Pre-requisites:

- Intro to Spatial Data
- Data Collection

#### Objectives:

- Understanding how to wrangle or prepare your data
- Recognize common methods to sptail data wrangling

#### Data:

iRods access: <br>&nbsp;&nbsp;&nbsp;``/iplant/home/shared/aegis/Spatial-bootcamp/data-hygiene/data-formatting``

- [US state boundaries (USA_adm1.shp](http://de.iplantcollaborative.org/dl/d/A80AF5AA-C1FF-4487-AD9B-846E1429F908/USA_adm1.zip)
- [US West GTOPO30 DEM (us_northwest_gt30w140n90.tif)](http://de.iplantcollaborative.org/dl/d/DD12B0DE-9DDE-4E13-90A3-240BC4DC1C5E/us_northwest_gt30w140n90.tif.zip)

----

{% include JB/setup %}
# Data Formatting

Data manipulation can be a simple endeavour if the data inputs come in the same or compatible formats. Data sources will usually have a specific format they use for their data which can make it easy to use work with their datasets. Unfortunately, when we gather data from various sources, we are usually given several different formats to data to deal with. Data wrangling is the process of manipulating and reformatting disparate data sources into compatible datasets. Data wrangling makes proper analysis/modeling possible. There are several methods common to spatial data wrangling some of which are:

 * Reprojection - Conversion from one projection coordinate system to another
 * Rasterization/Vectorization - Conversion from raster dataset to vector and vice versa
 * Extent Definition - Delineates the bounding box for a dataset, all data outside the box will become null
 * Reclassification - Assigns categorical values to a field based on a the contained values
 * Spatial Joins - Merges datasets based on spatial proximity/overlay

These methods can be performed using a variety of tools depending on the specific needs of the dataset. Here are a few typical choices to aide in data wrangling:

<br>

----

## Exercise

<ol>
<li>Open QGIS and set projection projection to EPSG:2927:<br><br>
As a reminder, before starting any project in a GIS program, you should first set the project projection to make sure your data comes in with the same extent. <br>If you don't set the project's projection, the program will use the projection of the first layer added or in the case for QGIS, EPSG:4326.<br><br>Project CRS: <em>Menu Bar > Project Properties > CRS</em><br><br><strong>Enable 'on the fly' CRS transformation</strong>. Search for <strong>2927</strong>.<br><br>Select <strong>NAD83(HARN) / Washington South (ftUS) EPSG:2927</strong> and click <strong>APPLY</strong> then <strong>OK</strong>.<br><br>
<img alt="projection" src="{{BASE_PATH}}{{ASSET_PATH}}/images/qgis-projection.png" class="screen-shot" /><br><br>
</li>
<li>Import US states shapefile:<br>
<ol>
<li>iRods access: <br>&nbsp;&nbsp;&nbsp;<code>/iplant/home/shared/aegis/Spatial-bootcamp/data-hygiene/data-formatting</code><br><br>
Or download here and <strong>Add Vector Layer</strong> <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/add-vector.png"/>:<br>&nbsp;&nbsp;&nbsp;<a href="link-here">US states boundaries (USA_adm1.shp)</a><br><br>You should now be viewing the US states boundaries in EPSG:2927:<br><br>
<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/usa-states.png" alt="USAStates" src="{{BASE_PATH}}{{ASSET_PATH}}/images/usa-states.png" class="screen-shot" />
</li>
</ol>
<li>Extract Washington state boundary:<br><br>
Since our project is directed at the state of Washington. We should extract the Washington state boundary for our study. The GADM[^7] project provides high-quality boundary data on country, state, and county levels. We can use the US-state level dataset to get the Washington boundary.<br><br>
<ol>
<li>Enable <strong>Select Feature</strong> tool: <em>Meun Bar > View > Select > Select Single Feature</em>. Or =, click on <strong>Select Features by area or single click</strong> tool <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/select-feature.png"/> and click on Washington to add to our selection.<br><br><img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/washington-selected.png" alt="washington-selected" src="{{BASE_PATH}}{{ASSET_PATH}}/images/washington-selected.png" class="screen-shot" />
</li>
<li>
Right click on the USA_adm1 layer and select <em>Save Selection As...</em><br>
<ul>
<li>Format: ESRI Shapefile </li>
<li>Save as: washington.shp</li>
<li>Encoding: UTF-8</li>
<li>CRS: <em>Browse</em> > NAD83(HARN)/Washington South (ftUS) EPSG:2927</li>
</ul><br><br>
<img alt="saveWashington" src="{{BASE_PATH}}{{ASSET_PATH}}/images/save-washington.png" class="screen-shot" />
</li>
</ol>
</li>
</li>
</ol>

<hr>
<!--
1. Set project projection to EPSG:2927<br><br> 
2. Add the US States shapefile<br><br>
  <img alt="USAStates" src="{{BASE_PATH}}{{ASSET_PATH}}/images/usa-states.png" class="screen-shot" />
3. Create a layer for the state of Washington 
  * Click on the state of Washington to select it<br>
  <img alt="washington-selected" src="{{BASE_PATH}}{{ASSET_PATH}}/images/washington-selected.png" class="screen-shot" />
  *       - Format: ESRI Shapefile
      - Save as: washington.shp
      - Encoding: UTF-8
      - CRS: 
          + Selected CRS
          + Browse > *NAD83(HARN)/Washington South (ftUS) EPSG:2927*<br>
-->

###### 4. Load the new washington layer<br>
  * Select *Add Vector Layer*
  * Browse to the new washington layer and click *Open*<br>

###### 5. Remove the USA_adm1 layer from the project <br>
  Now that we have the Washington layer, we can get rid of the U.S. layer. <br>
  * Right click the layer in the table of contents
  * Select *Remove*

###### 6. Add the DEM layer from the iPlant data store<br>
  DEMs or *Digital Elevation Models* are very useful for establishing topological context in a map. DEMs generally come as a raster dataset that consists of elevation values. There are several different byproducts that can be created from DEMs.<br><br>
  This DEM was taken from the GTOPO30 satellite data.<br>
  * In the left toolbar, select *Add Raster Layer*
  * Select *source_files/W140N90.DEM*<br>
  <img alt="demLoad" src="{{BASE_PATH}}{{ASSET_PATH}}/images/dem-load.png" class="screen-shot" />

###### 7. Project the dem to EPSG:2927
  * In the top menu, go to Raster > Projections > Warp (Reproject)
  * Use the following options
    + Input File: *W140N90*
    + Output File: *secondary_files/dem-project.tif*
    + Source SRS: *EPSG:4326*
    + Target SRS: **2927**<br> &nbsp;&nbsp;&nbsp;&nbsp;**NOTE**: When searching for an SRS, you MUST click on SRS in the bottom list otherwise you will not actually be selecting an SRS
    + Resampling Method: *Near*
    + No data values: *0*
  * Be sure to click CLOSE and not OK when exiting tools. Clicking OK will rerun the tool.
    <img alt="projectDEM" src="{{BASE_PATH}}{{ASSET_PATH}}/images/project-dem.png" class="screen-shot" />

###### 8. Save these files. We'll be using them in later lessons.
