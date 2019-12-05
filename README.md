# Flint-Water-Lead
A spatial analysis of water lead results in Flint, Michigan

# Overview

This project analyzed water lead tests collected during the Flint water crisis. The water samples were taken between 2015 and 2017 and were published by the Michigan Department of Health and Human Services. Samples were collected at schools, establishments, and sentinel stations (see histogram). I analyzed how the racial composition of households, estimated home values, and age of home structures were correlated with the percentage of water samples with lead levels above the federal action level of 15 parts per billion (ppb).

# Methods

I geocoded 11,756 water tests to a latitude and longitude. To estimate lead results for the city of Flint, I used Inverse Distance Weighting (IDW) Interpolation and Zonal Statistics to obtain the mean level of lead at the tract level.

I obtained demographic variables from the 2017 American Community Survey at the census tract level. Then, I calculated the percentage of water samples within each tract that were greater than or equal to 15 ppb. I created a correlation matrix of high lead levels and census data using R statistical software. Finally, I created a map showing the percent of tract covered by lead parcels using a shapefile provided by the University of Michigan, Flint.


# Conclusions

Positive correlations:
There are moderate positive correlation (0.3) between having lead levels above 15ppb and the number of white households in a tract, as well as the total number of households in a tract (0.19). The correlation between the percentage of the population living below poverty and the percentage of high lead levels in a tract is 0.1.

Negative correlations:
There are moderate negative correlations between the number of houses valued below $10,000 (-0.22) and high lead levels. There is a moderate negative correlation between high lead levels and the age of the housing structure (-0.21). This means that tracts with older homes were more likely to have high lead levels.
