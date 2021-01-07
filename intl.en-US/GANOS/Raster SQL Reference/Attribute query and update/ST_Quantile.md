# ST\_Quantile

This function queries the pixel values of the quantiles for a raster object.

## Prerequisites

The quantiles of the raster object are calculated by using the [ST\_StatsQuantile]() function.

## Syntax

```
setof record ST_Quantile(raster raster_obj,
                   float8[] quantiles default NULL,
                   cstring bands default '',
                   boolean exclude_nodata_value default true, 
                   out integer band,
                   out float8 quantile,
                   out float8 value)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|
|quantiles|The quantiles whose pixel values you want to calculate. Valid values: 0.25, 0.5, and 0.75. You can specify one or more quantiles.|
|bands|The serial numbers of the bands based on which the pixel values of the quantiles are calculated. Supported formats are `'0-2'` and `'1,2,3'`. Serial numbers start from 0. Default value: empty string \(`''`\). The default value specifies all bands.|
|exclude\_nodata\_value|Specifies whether to include NoData values during calculation.|
|band|Specifies to return the serial numbers of the bands.|
|quantile|Specifies to return the quantiles.|
|value|Specifies to return the pixel values.|

## Examples

```
-- Calculate the pixel value of the 0.25 quantile based on all bands.
SELECT  (ST_Quantile(rast, ARRAY[0.25], '0-2', true)). * FROM rat_quantile WHERE id = 1;
 band | quantile | value 
------+----------+-------
    0 |     0.25 |    11
    1 |     0.25 |    10
    2 |     0.25 |    50
(3 rows)

-- Calculate the pixel values of the 0.25, 0.5, and 0.75 quantiles based on band 0.
SELECT  (ST_Quantile(rast, NULL, '0', true)). * FROM rat_quantile WHERE id = 1;
 band | quantile | value 
------+----------+-------
    0 |     0.25 |    11
    0 |      0.5 |    11
    0 |     0.75 |    65
(3 rows)
```

