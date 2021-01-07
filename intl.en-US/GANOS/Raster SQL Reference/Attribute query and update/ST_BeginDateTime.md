# ST\_BeginDateTime

This topic describes the ST\_BeginDateTime function, which obtains the start time of a raster.

## Syntax

```
timestamp ST_BeginDateTime(raster raster_obj);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster whose start time you want to obtain.|

## Examples

```
SELECT ST_beginDateTime(raster_obj)
FROM raster_table;

    st_begindatetime     
-------------------------
Wed Jan 01 00:00:00 2020 
```

