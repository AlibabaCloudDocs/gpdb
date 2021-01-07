# ST\_ChunkWidth

This function returns the tile width of the specified raster object.

## Syntax

```
integer ST_ChunkWidth(raster raster_obj)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|

## Description

This function returns the tile width of the specified raster object. The width is measured in pixels.

## Examples

```
select ST_ChunkWidth(rast)
from raster_table;

 st_chunkwidth  
----------
      256 
```

