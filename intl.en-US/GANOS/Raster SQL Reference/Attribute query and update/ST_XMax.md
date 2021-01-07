# ST\_XMax

This function returns the maximum X-coordinate value of the specified raster object.

## Syntax

```
float8 ST_XMax(raster raster_obj)
float8 ST_XMax(raster raster_obj, integer pyramid)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|
|pyramid|The pyramid level. The value starts from 0.|

## Description

If the raster object has been georeferenced, this function returns the maximum X-coordinate value of the raster object. Otherwise, this function returns -1.

## Examples

```
select ST_XMax(rast)
from raster_table;

 st_xmax  
----------
      121 
```

