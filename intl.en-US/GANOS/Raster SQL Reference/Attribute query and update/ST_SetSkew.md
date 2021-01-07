# ST\_SetSkew

Configures the skews of a raster object on the X and Y scales in the spatial reference system.

## Syntax

```
raster ST_SetSkew(raster raster_obj,  float8 skewX, float8 skewY) 
raster ST_SetSkew(raster raster_obj,  float8 skewXY)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|
|skewX|The skew of the raster object on the X scale.|
|skewY|The skew of the raster object on the Y scale.|
|skewXY|The skew of the raster object on the X and Y scales. This parameter specifies the same skew on the X and Y scales.|

## Examples

```
UPDATE raster_table
SET rast = ST_SetSkew(rast, 0.3, 0.6)
WHERE id = 2;

select st_skewx(rast), st_skewy(rast)
from raster_table
WHERE id = 2;

 st_skewx | st_skewy 
----------+-----------
      0.3 |      0.6
```

