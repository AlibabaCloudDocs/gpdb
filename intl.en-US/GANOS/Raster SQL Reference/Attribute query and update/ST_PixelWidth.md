# ST\_PixelWidth

Queries the pixel width of a raster object in the spatial reference system.

## Syntax

```
float8 ST_PixelWidth(raster raster_obj)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|

## Description

The red part in the following figure shows the pixel width of the raster object. If the skew of the raster object is 0, the pixel width is equal to the X scale.

![ST_PixelWidth](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7919209951/p88911.png)

## Examples

```
select st_pixelwidth(rast), st_pixelheight(rast)
from raster_table;

 st_pixelwidth | st_pixelheight 
---------------+----------------
            60 |             60
```

