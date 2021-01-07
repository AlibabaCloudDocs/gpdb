# ST\_BestPyramidLevel

This function computes the best pyramid level based on the world space, width, and height of a viewport.

## Syntax

```
integer ST_BestPyramidLevel(raster rast, Box extent, integer width, integer height);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|rast|The raster object.|
|box|The world space of the viewport, in the format of `((minX,minY),(maxX,maxY))`.|
|width|The width of the viewport, in pixels.|
|height|The height of the viewport, in pixels.|

## Description

The raster object must have a valid spatial reference system identifier \(SRID\).

## Examples

```
Select ST_BestPyramidLevel(raster_obj, '((128.0, 30.0),(128.5, 30.5))', 800, 600) from raster_table where id = 10;

---------------------
3
```

