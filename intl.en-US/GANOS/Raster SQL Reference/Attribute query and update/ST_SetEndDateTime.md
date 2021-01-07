# ST\_SetEndDateTime

This topic describes the ST\_SetEndDateTime function, which specifies the end time of a raster.

## Syntax

```
raster ST_SetEndDateTime(raster raster_obj, timestamp time);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster whose end time you want to specify.|
|time|The end time of the raster. We recommend that you specify the time in the `yyyy-MM-dd HH:mm:ss` format. Example: `2020-08-30 18:00:00`.|

## Examples

```
SELECT ST_endDateTime(ST_setEndDateTime(raster_obj, '2020-01-01'))
FROM raster_table;

    st_enddatetime     
-------------------------
Wed Jan 01 00:00:00 2020                                                                                
```

