# ST\_CellDepth

This function returns the pixel depth of a raster object. The pixel depth can be 0, 1, 2, 4, 8, 16, 32, or 64, where 0 indicates that the pixel depth is unknown.

## Syntax

```
integer ST_CellDepth(raster raster_obj);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster object.|

## Examples

```
select ST_CellDepth(raster_obj) from raster_table;

__________________________________
8
```

