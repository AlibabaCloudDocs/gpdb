# ST\_YMax

This topic describes the ST\_YMax function, which obtains the maximum Y coordinate of a raster.

## Syntax

```
float8 ST_YMax(raster raster_obj)
float8 ST_YMax(raster raster_obj, integer pyramid)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The raster whose maximum Y coordinate you want to obtain.|
|pyramid|The pyramid level at which the raster resides. Valid values start from 0.|

## Description

If the raster is georeferenced, this function returns the maximum Y coordinate of the raster. Otherwise, this function returns a value that is equal to the height of the raster minus 1.

## Example

```
select st_ymax(rast)
from raster_table;

 st_ymax  
----------
      31 
```

