# A spatial analysis of water lead results in Flint, Michigan

## Overview

This project analyzed water lead tests collected during the Flint water crisis. The water samples were taken between 2015 and 2017 and were published by the Michigan Department of Health and Human Services. Samples were collected at schools, establishments, and sentinel stations (see histogram). I analyzed how the racial composition of households, estimated home values, and age of home structures were correlated with the percentage of water samples with lead levels above the federal action level of 15 parts per billion (ppb).

![alt text](https://github.com/ilsep93/Geospatial-Lead-Results/blob/master/Paniagua_URP520_Poster.pdf)

## Methods

### Adding stand-alone table to ArcGIS

The .xlsx dataset was imported to ArcGIS through the Add Data dialog box.

### Geoprocessing stand-alone table

I geocoded the “NAME” key field in the stand-alone table using GeocodeAddress under the Geocoding toolbox. The input table was the stand-alone table, the address locator was the World Atlas locator, the address fields were NAME and ZIPCODE. 11,756 records out of 12,626 were successfully geolocated.

### Added web maps online portal

The following Web maps, created by the University of Michigan-Flint using American Community Survey data, were added using the online portal in the Catalog:
“Map Flint - 2017 Flint by tract ACS5YR Housing Value”
“Map Flint - 2017 Flint by tract ACS5YR Median Household Income”
“Map Flint - 2017 Flint by tract ACS5YR Year Structure Built”
“Map Flint - 2017 Flint by tract ACS5YR Ratio Income to Poverty”

The Web maps were combined using the Add Join tool under the Joins and Relates toolbox under the Input Join Field “Tract.” The output Join Field was a layer called “ACS_Flint.”

### Clipping

Because some water testing was conducted outside the city limits of Flint, the clip tool in the analysis toolbox and the extract toolset was used to only retain water quality records inside the city. Input layer was the geocoded data quality points. The clip features was the Flint census tract polygon layer.

### Interpolation

I used Inverse Distance Weighting (IDW) Interpolation under the Spatial Analyst Tools toolbox and the Interpolation toolset to estimate lead results for the city of Flint.

The input features were the clipped, geolocated data quality points. The Z value field was Results_Lead. The output raster was “idw_lead”. The output cell size was “5”. The power was 2, the search radius was variable and the number of points was 12.  The Mask option was the ACS Flint polygon layer.

### Zonal statistics

To obtain averages for each census tract, I used the Zonal Statistics tool under the Spatial Analyst Tools toolset and the Zonal toolbox. The input raster was the “ACS_Flint” polygon layer, the Zone field was “Tract”, the input value raster was “idw_lead”, the Output raster was “MeanLead,” and the Statistics type was “Mean.”

For map’s symbology, I used the Manual Interval method that corresponded to the lead categories: 1-4, 5-14, 15-49, 50-149, and 150 and above.

### Point density

I created a map showing the point density of the water quality points using the Point Density tool under the Spatial Analyst toolbox and the Density toolset. The Input point features was water quality points layer; Population field was none, Output raster was “point_density”, Output cell size was 2E-04; Radius was 20; Neighborhood was circle; Area units was square map units; Mask was ACS_Flint polygon layer.

I symbolized the points using unique values according to their type of location (school, establishment, or sentinel).

### Percentage of population below poverty calculated field

To create a new variable for the percentage of the population under poverty, I first added a new field for “Below Poverty” under Data and Fields.
I created a calculated field under Data Management toolbox and the Fields toolset. 
The Input table was “ACS_Flint”, the  Field Name was Below Poverty

The expression was (!E_LT0p5! + !E_0p5_1! /!TotalPop!) * 100, or the estimated number of households with income to poverty ratio below 1 divided by the estimated total population for that tract multiplied by 100.

Below poverty was visualized using graduated colors using the geometric interval method, with five classes.

### Percent of tract covered by lead pipes

I obtained a shapefile from the University of Michigan-Flint containing the known location of lead pipes as of November 2016. I added the shapefile to ArcGIS using the Catalog pane.

I used the Select Layer by attribute tool under the Layers and Table Views toolbox. The input rows were “SL_Conn_11_7”, the selection type was SQL statement “Curr_Conn LIKE '%Lead%” (connections that contained Lead or Lead-Copper).I created a new polygon layer using the highlighted records named “LeadPipes.”

To combine the ACS_Flint and LeadPipes layers, I performed a Spatial Join using the Analysis Tools toolbox and the Overlay toolset. The target features was LeadPipes, the Join Features was “ACS_Flint”, the Join Operation was “one to many”, the Match Option was “Intersect”. 

The input table was “LeadPipes_SpatialJoin”, the Field Name was “SumLead”, the formula was “!Shape_Area! /!Tract_Area!”. This provides the percentage of tract covered by lead pipes, at an individual level.

I used the Summary Statistics tool under the Analysis toolbox and the Statistics toolset to calculate the total area covered by lead tracts per tract.

## Conclusions

Positive correlations:
There are moderate positive correlation (0.3) between having lead levels above 15ppb and the number of white households in a tract, as well as the total number of households in a tract (0.19). The correlation between the percentage of the population living below poverty and the percentage of high lead levels in a tract is 0.1.

Negative correlations:
There are moderate negative correlations between the number of houses valued below $10,000 (-0.22) and high lead levels. There is a moderate negative correlation between high lead levels and the age of the housing structure (-0.21). This means that tracts with older homes were more likely to have high lead levels.
