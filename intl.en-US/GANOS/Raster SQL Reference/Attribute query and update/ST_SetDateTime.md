# ST\_SetDateTime

This function specifies the start and end time of a raster or the time information of a band.

## Syntax

```
raster ST_setDateTime(raster raster_obj,
                      timestamp start,
                      timestamp end);
raster ST_setDateTime(raster raster_obj,
                      integer band,
                      timestamp time);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster whose start and end time you want to specify.|
|band|The sequence number of the band whose start and end time you want to specify. Valid values start from 0.|
|time|The time information of the specified band. We recommend that you specify the time in the `yyyy-MM-dd HH:mm:ss` format. Example: `2020-08-30 18:00:00`.|
|start|The start time that you want to specify. We recommend that you specify the time in the `yyyy-MM-dd HH:mm:ss` format.|
|end|The end time that you want to specify. We recommend that you specify the time in the `yyyy-MM-dd HH:mm:ss` format.|

## Description

The SetDateTime function returns the customized metadata items of a raster.

## Examples

```
select ST_DateTime(ST_setDateTime(raster_obj, 0, '2020-01-02'::timestamp), 0)
FROM raster_table

       st_datetime        
--------------------------
 Thu Jan 02 00:00:00 2020
```

