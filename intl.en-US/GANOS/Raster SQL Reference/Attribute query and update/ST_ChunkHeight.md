# ST\_ChunkHeight

This function returns the tile height of the specified raster object.

## Syntax

```
integer ST_ChunkHeight(raster raster_obj)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|

## Description

This function returns the tile height of the specified raster object. The tile height is measured in pixels.

## Examples

```
select ST_ChunkHeight(rast)
from raster_table;

 st_chunkheight  
----------
      256 
```

