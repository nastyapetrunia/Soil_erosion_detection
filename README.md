# Soil_erosion_detection
## Description 
Model for detecting soil erosion using geospatial data and imagery. 
## Analysis
Given dataframe consists of 935 rows and 7 columns
&nbsp;

![datafrae info](Images/dataframe_info.png)
&nbsp;

**Does every data point _actually_ represent soil erosion case?**
&nbsp;

![c_d_c](Images/code_description_count.png)
&nbsp;

Simply relying on dataset makes it not obvious, so a better way to find out is to actually visualise given polygons. At first, lets look at them separately.
&nbsp;

![polygons](Images/raw_polygons.png)
&nbsp;

The original raster image size is (10980, 10980, 3), and plotted polygons make a rectangle instead of a square. So some of our polygons may be lost during masking. 
&nbsp;

Masking files with rasterio returns:
&nbsp;

>Rasterio failed to mask 435 files
&nbsp;

So after aliminating the 'out of bounds' polygons we're left with 500 rows of data.
&nbsp;

*Most of the 'labeled' data is out of raster's bounds so it's lost.*
&nbsp;

![masked and failed](Images/masked_and_failed.png)
&nbsp;

Now plotting polygons over raster:
&nbsp;

![plotted_masked](Images/1raster.png) ![plotted_masked](Images/1masked.png) 
&nbsp;

As can be seen on the images, some parts of soil with erosion are not coverred with polygons, but all polygons seem to represent soil erosion case.
