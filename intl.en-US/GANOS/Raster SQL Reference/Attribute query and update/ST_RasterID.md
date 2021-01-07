# ST\_RasterID

This function returns the universally unique identifier \(UUID\) of a raster object.

## Syntax

```
text ST_RasterID(raster raster_obj);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster object.|

## Examples

```
select ST_RasterID(raster_obj) from raster_table;
------------------------------------
4e692ed0-74e2-42a3-a10d-c28d4ae31982
```

