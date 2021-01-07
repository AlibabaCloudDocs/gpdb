# ST\_YMin

This function returns the minimum Y-coordinate value of the specified raster object.

## Syntax

```
float8 ST_YMin(raster raster_obj)
float8 ST_YMin(raster raster_obj, integer pyramid)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|
|pyramid|The pyramid level. The value starts from 0.|

## Description

If the raster object has been georeferenced, this function returns the minimum Y-coordinate value of the raster object. Otherwise, this function returns 0.

## Examples

```
select ST_YMin(rast)
from raster_table;

 st_ymin  
----------
      30 
```

