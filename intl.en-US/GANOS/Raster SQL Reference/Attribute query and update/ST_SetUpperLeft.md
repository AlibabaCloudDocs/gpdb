# ST\_SetUpperLeft

Configures the upper-left X and Y coordinates of a raster object in the spatial reference system.

## Syntax

```
raster ST_SetUpperLeft(raster raster_obj,  float8 upperleftX, float8 upperleftY) 
raster ST_SetUpperLeft(raster raster_obj,  float8 upperleftXY)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|
|upperleftX|The upper-left X coordinate of the raster object in the spatial reference system.|
|upperleftY|The upper-left Y coordinate of the raster object in the spatial reference system.|
|upperleftXY|The upper-left X and Y coordinates of the raster object in the spatial reference system. This parameter specifies the same upper-left X and Y coordinates.|

## Examples

```
UPDATE raster_table
SET rast = ST_SetUpperleft(rast, 120, 30)
WHERE id = 2;

select st_upperleftx(rast), st_upperlefty(rast)
from raster_table
WHERE id = 2;

 st_upperleftx | st_upperlefty 
---------------+---------------
          120  |            30
```

