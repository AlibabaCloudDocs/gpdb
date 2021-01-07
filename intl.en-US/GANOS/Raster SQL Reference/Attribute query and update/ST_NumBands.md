# ST\_NumBands

This function returns the number of bands in a raster object.

## Syntax

```
integer ST_NumBands(raster raster_obj);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster object.|

## Examples

```
select ST_NumBands(raster_obj) from raster_table;
------------------------------------
3
```

