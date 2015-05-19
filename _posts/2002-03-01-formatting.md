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

<br>

Download files:

- [US state boundaries (USA_adm1.shp](http://de.iplantcollaborative.org/dl/d/A80AF5AA-C1FF-4487-AD9B-846E1429F908/USA_adm1.zip)
- [US Northwest GTOPO30 DEM (gt30w140n90_us_northwest.tif)](http://de.iplantcollaborative.org/dl/d/57947D2C-2705-41B3-8190-6F130139BAEC/gt30w140n90_us_northwest.tif)

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

This next simple data wrangling exercise will walk through extracting a feature from a shapefile and reprojecting (warpping) a raster. Two commonly used processes when working with spatial data.

<br><br>

<ol>
<li>Open QGIS and set projection projection to EPSG:2927:<br><br>
As a reminder, before starting any project in a GIS program, you should first set the project projection to make sure your data comes in with the same extent. <br>If you don't set the project's projection, the program will use the projection of the first layer added or in the case for QGIS, EPSG:4326.<br><br>Project CRS: <em>Menu Bar > Project Properties > CRS</em><br><br><strong>Enable 'on the fly' CRS transformation</strong>. Search for <strong>2927</strong>.<br><br>Select <strong>NAD83(HARN) / Washington South (ftUS) EPSG:2927</strong> and click <strong>APPLY</strong> then <strong>OK</strong>.<br><br>
<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/formatting-1.png" alt="projection" src="{{BASE_PATH}}{{ASSET_PATH}}/images/formatting-1.png" class="screen-shot" /><br><br>
</li>
<li>Import US states shapefile:<br>
<ol>
<li>iRods access: <br>&nbsp;&nbsp;&nbsp;<code>/iplant/home/shared/aegis/Spatial-bootcamp/data-hygiene/data-formatting</code><br><br>
Or download here and <strong>Add Vector Layer</strong> <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/add-vector.png"/>:<br>&nbsp;&nbsp;&nbsp;<a href="link-here">US states boundaries (USA_adm1.shp)</a><br><br>You should now be viewing the US states boundaries in EPSG:2927:<br><br>
<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/formatting-2.png" alt="USAStates" src="{{BASE_PATH}}{{ASSET_PATH}}/images/formatting-2.png" class="screen-shot" />
</li>
</ol>
</li>
<li>Extract Washington state boundary:<br><br>
Since our project is directed at the state of Washington. We should extract the Washington state boundary for our study. The GADM[^7] project provides high-quality boundary data on country, state, and county levels. We can use the US-state level dataset to get the Washington boundary.<br><br>
<ol>
<li>Enable <strong>Select Feature</strong> tool: <em>Meun Bar > View > Select > Select Single Feature</em>. Or =, click on <strong>Select Features by area or single click</strong> tool <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/select-feature.png"/> and click on Washington to add to our selection.<br><br><img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/formatting-3.png" alt="washington-selected" src="{{BASE_PATH}}{{ASSET_PATH}}/images/formatting-3.png" class="screen-shot" />
</li>
<li>
<em>Right click USA_adm1 (layer list) >  Save As...</em><br>
<ul>
<li>Format: ESRI Shapefile </li>
<li>Save as: washington.shp</li>
<li>CRS: <em>Browse</em> > NAD83(HARN)/Washington South (ftUS) EPSG:2927</li>
<li>Encoding: UTF-8</li>
<li>Save only selected feature: (checked)</li>
<li>Add saved file to map: (checked)</li>
</ul><br><br>
<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/formatting-4.png" alt="saveWashington" src="{{BASE_PATH}}{{ASSET_PATH}}/images/formatting-4.png" class="screen-shot" />
</li>
</ol>
</li>
<li>Remove USA_adm1 layer from the project:<br>
We no longer need the entire USA boundaries now that we have extracted Washington.<br><br>
<em>Right-click USA_adm1 (layer list) > Remove</em><br><br>
You should now only have the Washington state boundary in your project<br><br>Zoom to layer: <em>Right-click layer (layer list) > Zoom to Layer</em><br><br><img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/formatting-step-4.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/formatting-step-4.png" alt="Spatial Data Bootcamp"/>
</li>
<li>Import the US northwest GTOPO30 DEM from the iPlant data store (throught iRods):<br>
<ol>
<li>iRods access: <br>&nbsp;&nbsp;&nbsp;<code>/iplant/home/shared/aegis/Spatial-bootcamp/data-hygiene/data-formatting</code><br><br>
Or download here and <strong>Add Raster Layer</strong> <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/add-raster.png"/>:<br>&nbsp;&nbsp;&nbsp;<a href="http://de.iplantcollaborative.org/dl/d/57947D2C-2705-41B3-8190-6F130139BAEC/gt30w140n90_us_northwest.tif">US northwest GTOPO30 DEM (gt30v140n90_us_northwest.tif)</a><br><br>The DEM transforms to EPSG:2927 since we had enabled 'on-the-fly' transformations at the beginning of our workflow. Notice the DEM value -9999, this represents no-data.<br><br>Move the washington layer above the DEM from within the layer list: <em>Click-hold-drag washington to the top of the layer list</em><br><br>Zoom to washington layer: <em>Right-click washington (layer list) > Zoom to Layer</em><br><br>We are now visualizing the boundary of Washington on top of an elevation raster<br><br>
<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/formatting-5.png" alt="USAStates" src="{{BASE_PATH}}{{ASSET_PATH}}/images/formatting-5.png" class="screen-shot" />
</li>
</ol>
</li>
<li>Project the DEM to EPSG:2927<br><br>
We are able to visualize the DEM in EPSG:2927 thanks to our 'on-the-fly' transformation in QGIS. However if we need to store the DEM with EPSG:2927 we must <em>reproject, or warp</em> our raster.<br><br>Open the raster <strong>Warp (Reproject)</strong> tool: <em>Menu Bar > Projections > Warp (Reproject)</em><br><br>Configure the inputs as follows:<br>
<ul>
<li>Input file: <strong>gt30w140n90_us_northwest</strong></li>
<li>Output file: <strong>dem_2927.tif</strong></li>
<li>Source: <strong>EPSG:4326</strong></li>
<li>Target: <strong>EPSG:2927</strong></li>
<li>Resampling Method: <strong>Near</strong></li>
<li>No data values: <strong>0</strong></li>
<li>We need to add a custom parameter into <a href="http://www.gdal.org/gdalwarp.html" target="_blank">gdalwarp</a>: <strong>Target Resolution</strong><br><br>
By default, GTOPO30 is provided in <em>meters</em>, while <a href="http://spatialreference.org/ref/epsg/2927/" target="_blank">NAD83(HARN) / Washington South (ftUS)</a> (EPSG:2927) is in <em>US survey feet</em>. Rasters <em>must</em> have the <em>same</em> resolution for an analysis to be valid.<br><br>
Next to the code at the bottom of the window, <em>Click the <strong>Edit tool</strong> <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/gdalwarp-edit.png"/> icon </em>to enable editing the code:<br><br>
It's best practice to declare a target resolution so we understand what we're working with. Below, we have calculated feet per kilometer: 3280 feet = 1 kilometer. Even though our projection is in US survey feet, we are still working with kilometers.<br><br>
Add the following code (see the example below): <code>-tr 3280 3280</code><br><br><strong>Do not disable editing once finished</strong>. This will revert changes to the code and our target resolution will not be changed.<br><br>Click <strong>OK</strong> to <em>run</em> the tool, click <strong>CLOSE</strong> to <em>close</em> the tool.<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/gdalwarp-1.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/gdalwarp-1.png" alt="Spatial Data Bootcamp"/>
</li>
</ul>
<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/formatting-6.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/formatting-6.png" alt="Spatial Data Bootcamp"/>
</li>
<li>Confirm the transformation:<br>
Open a new project, there's no need to save this current project.<br><br>
Open the reprojected, or warpped DEM<br><br>
The default projection should be EPSG:2927 (without 'on-the-fly' transformation), assuming everything went well.<br><br>
You could also measure the pixels with the <strong>Measure tool</strong> <img src="{{BASE_PATH}}{{ASSET_PATH}}/images/measure-tool.png"/>.<br><br>
<img data-featherlight="{{BASE_PATH}}{{ASSET_PATH}}/images/formatting-7.png" src="{{BASE_PATH}}{{ASSET_PATH}}/images/formatting-7.png" alt="Spatial Data Bootcamp" />
</ol>

<p>You should be able to: extract a feature from a shapefile; and warp (reproject) a raster while declaring a target resolution. Close the project without saving. <strong>dem_2927</strong> will be used in the <strong>Landslide Exercise</strong>.</p>

