# ST\_SetScale

Configures the pixel widths of a raster object on the X and Y scales in the spatial reference system.

## Syntax

```
raster ST_SetScale(raster raster_obj,  float8 scaleX, float8 scaleY) 
raster ST_SetScale(raster raster_obj,  float8 scaleXY)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|
|scaleX|The pixel width of the raster object on the X scale in the spatial reference system.|
|scaleY|The pixel width of the raster object on the Y scale in the spatial reference system.|
|scaleXY|The pixel width of the raster object on the X and Y scales in the spatial reference system. This parameter specifies the same pixel width on the X and Y scales.|

## Examples

```
UPDATE raster_table
SET rast = ST_SetScale(rast, 1.3, 0.6)
WHERE id = 2;

select st_scalex(rast), st_scaley(rast)
from raster_table
WHERE id = 2;

 st_scalex | st_scaley 
-----------+-----------
       1.3 |       0.6
```

