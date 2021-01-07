# S​T\_ClipDimension

This function computes the raster space of a clipped area.

## Syntax

```
box ST_ClipDimension(raster raster_obj, integer pyramidLevel, box extent);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster object.|
|pyramidLevel|The pyramid level.|
|box|The world space of the clipped area.|

## Description

The raster object must have a valid spatial reference system identifier \(SRID\).

## Examples

```
Select ST_ClipDimension(raster_obj, 2, '((128.0, 30.0),(128.5, 30.5))') from raster_table where id = 10;

------------------------------------
'((200, 300),(600, 720))'
```

