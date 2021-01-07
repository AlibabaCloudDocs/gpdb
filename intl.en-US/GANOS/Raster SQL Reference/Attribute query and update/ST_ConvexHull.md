# ST\_ConvexHull

This topic describes the ST\_ConvexHull function, which obtains the minimum convex geometry that encloses all geometries within a raster.

## Syntax

```
geometry ST_ConvexHull(raster source);
geometry ST_ConvexHull(raster source, 
                       integer pyramid);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|source|The raster whose minimum convex geometry you want to obtain.|
|pyramid|The pyramid level at which the raster resides. Valid values start from 0. Default value: 0.|

## Description

This function obtains the minimum convex geometry that encloses all geometries within a raster.

![Minimum convex geometry](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/1280289951/p129345.png)

## Example

```
SELECT st_astext(st_convexhull(rast_object))
FROM raster_table;

----------------------------------------------------
 POLYGON((-180 90,180 90,180 -90,-180 -90,-180 90))

--Specify the pyramid level at which the raster resides.                 
SELECT st_astext(st_convexhull(rast_object,  1))
FROM raster_table;

----------------------------------------------------
 POLYGON((-180 90,180 90,180 -90,-180 -90,-180 90)) 
```

