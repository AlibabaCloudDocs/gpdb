# ST\_Contains

This topic describes the ST\_Contains function. This function identifies the spatial relationship between two raster objects or between a raster object and a geometric object to determine whether the first specified object completely contains the second.

## Syntax

```
bool  ST_Contains(raster rast1, raster rast2);
bool  ST_Contains(raster rast, geometry geom);
bool  ST_Contains(geometry geom, raster rast);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|rast1|The raster object 1.|
|rast2|The raster object 2.|
|rast|A raster object.|
|geom|A geometric object.|

## Example

```
SELECT a.id
FROM tbl_a a, tbl_b b
WHERE ST_Contains(a.rast, b.rast)
```

