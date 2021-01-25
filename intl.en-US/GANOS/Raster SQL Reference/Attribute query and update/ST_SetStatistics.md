# ST\_SetStatistics

This function sets the statistics about a specified band of a raster object.

## Syntax

```
raster ST_SetStatistics(raster rast, integer band, double min, double max, double mean, double std,cstring samplingParams);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|rast|A raster object.|
|band|The sequence number of the band, which starts from 0.|
|min,max,mean,std|The statistics.|
|samplingParams|The sampling parameters. Valid value: \{"approx":false\} and \{"approx":true\}.|

## Example

```
update rast set rast=ST_SetStatistics(rast,0,0.0 , 255.0, 125.0, 23.6, '{"approx":false}');

__________________________________
(1 row)
```

