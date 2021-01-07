# ST\_PixelHeight

This function queries the pixel height of a raster object in the spatial reference system.

## Prerequisites

The raster object has a valid spatial reference system identifier \(SRID\).

## Syntax

```
 float8 ST_PixelHeight(raster raster_obj)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|

## Description

The red part in the following figure shows the pixel height of the raster object. If the skew of the raster object is 0, the pixel height is equal to the Y scale.

![ST_PixelHeight](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7919209951/p88912.png)

## Examples

```
select st_pixelwidth(rast), st_pixelheight(rast)
from raster_table;

 st_pixelwidth | st_pixelheight 
---------------+----------------
            60 |             60
```

