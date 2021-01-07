# ST\_EndDateTime

This topic describes the ST\_EndDateTime function, which obtains the end time of a raster.

## Syntax

```
timestamp ST_EndDateTime(raster raster_obj);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster whose end time you want to obtain.|

## Examples

```
SELECT ST_endDateTime(raster_obj)
FROM raster_table;

     st_enddatetime      
-------------------------
Thu Jan 02 00:00:00 2020 
```

