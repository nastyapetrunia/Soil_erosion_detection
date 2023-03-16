# Soil_erosion_detection
## Description 
Model for detecting soil erosion using geospatial data and imagery. 
&nbsp;

### Table of content
[Analysis](#analysis)
&nbsp;

[Approach](#approach)
&nbsp;

[Model and Results](#model-and-results)
&nbsp;

[Proposals and Results of Research](#proposals-and-results-of-research)
## Analysis
Given dataframe consists of 935 rows and 7 columns
&nbsp;

![datafrae info](Images/dataframe_info.png)
&nbsp;

**Does every data point _actually_ represent soil erosion case?**
&nbsp;

![c_d_c](Images/code_description_count.png)
&nbsp;

Simply analysing dataset makes it not obvious, so a better way to find out is to actually visualise given polygons. At first, lets look at them separately.
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

As can be seen on the images, some parts of soil erosion are not coverred with polygons, but all polygons seem to represent soil erosion case.
## Approach
Considering the fact that dataset initially had less than a half of data labeled, and after getting polygons that are in raster bounds that number decreased to less than 20%, there are two possible options: to label data by hand (which would be hard to complete in time), or to use only image and polygons (which is what was desided to do).
&nbsp;

To create a dataset for this approach I created a binary mask for all polygons that are in raster bounds, cut both mask and raster into 4096 pieces, and resized images from (171, 171, 3) to (128, 128, 3). So resulting dataset had an original image and a corresponding binary mask (369 pairs). I also removed all images (and their masks) that did not have soil erosion (based on the binary mask).
&nbsp;

<sub>Images plotted before resizing</sub>
&nbsp;

![raster_cut](Images/raster_cut.png) ![masked_cut](Images/masked_cut.png) 
&nbsp;

## Model and Results
Created tf.keras model performs poorely, which is not really surprising. Possible ways to build a model that could perform well would be getting more data or using transfer learning (e.g. [Mask R-CNN](https://medium.com/@c_61011/transfer-learning-with-mask-r-cnn-f50cbbea3d29)).
&nbsp;

![model_accuracy](Images/model_accuracy.png) ![model_loss](Images/model_loss.png) 
&nbsp;

## Proposals and Results of Research
[As research shows](https://www.researchgate.net/publication/355448765_Satellite-Based_Soil_Erosion_Mapping) there are a lot for factors that affect soil erosion. Vegetation, farming, surface characteristics, rainfalls, wind, etcetera. If we are talking about building a model, that could predict future soil condition, we would need to analyse data over an extended peiod of time, monitoring changes. For those purpouses it would be important to detect crop canopy development throughout a growing season (as an indicator of erosion), and long-term growth of rill and gully formation.  It could be helpfull to maybe collect some weather data, for example from [here](https://www.worldweatheronline.com/kiev-weather-history/kyyivska-oblast/ua.aspx) (contains weather data from 2009 to 2023), analyse that data and maybe use it for better detection.
&nbsp;

If the purpouse of a model would be to state whether a particular area of soil has any erosion or not, than we could still use the stated approach, but we would no longer need to monitor how the condition of the soil changes over time - we would only need the data as it is.

