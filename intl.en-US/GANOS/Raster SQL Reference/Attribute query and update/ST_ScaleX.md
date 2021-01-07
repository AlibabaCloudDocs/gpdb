# ST\_ScaleX

Queries the pixel width of a raster object on the X scale in the spatial reference system.

## Prerequisites

The raster object has a valid spatial reference system identifier \(SRID\).

## Syntax

```
float8 ST_ScaleX(raster raster_obj)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|

## Examples

```
select st_scalex(rast), st_scaley(rast)
from raster_table;

 st_scalex | st_scaley 
-----------+-----------
       1.3 |       0.3
```

