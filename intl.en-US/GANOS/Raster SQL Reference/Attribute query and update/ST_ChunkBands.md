# ST\_ChunkBands

This function returns the number of bands for a tile of the specified raster object.

## Syntax

```
integer ST_ChunkBands(raster raster_obj)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|

## Examples

```
select ST_ChunkBands(rast)
from raster_table;

 st_chunkbands  
----------
      3
```

