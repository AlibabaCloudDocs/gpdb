# ST\_SetBeginDateTime

This topic describes the ST\_SetBeginDateTime function, which specifies the start time of a raster.

## Syntax

```
raster ST_SetBeginDateTime(raster raster_obj,timestamp time);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster whose start time you want to specify.|
|time|The start time of the raster. We recommend that you specify the time in the `yyyy-MM-dd HH:mm:ss` format. Example: `2020-08-30 18:00:00`.|

## Examples

```
SELECT ST_beginDateTime(ST_setBeginDateTime(raster_obj, '2020-01-01'))
FROM raster_table;

    st_begindatetime     
-------------------------
Wed Jan 01 00:00:00 2020 
```

