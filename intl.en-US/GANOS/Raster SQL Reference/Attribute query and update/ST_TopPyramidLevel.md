# ST\_TopPyramidLevel

This function returns the highest pyramid level of a raster object.

## Syntax

```
integer TopPyramidLevel(raster raster_obj);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster object.|

## Examples

```
select ST_TopPyramidLevel(raster_obj) from raster_table;

__________________________________
6
```

