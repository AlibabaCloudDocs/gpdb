# ST\_World2RastCoord

This function calculates the pixel coordinates of a cell by using an inverse affine transformation formula based on the world coordinates and pyramid level of the cell.

## Syntax

```
point ST_World2RastCoord(raster raster_obj, integer pyramidLevel, point coord);
geometry ST_World2RastCoord(raster raster_obj, integer pyramidLevel, geometry geom);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster object.|
|pyramidLevel|The pyramid level.|
|coord|The world coordinates of the cell.|
|geom|The geometry to be converted.|

## Description

The raster object must have a valid spatial reference system identifier \(SRID\).

A geometry will be returned. The x coordinate is the column number in raster and the y coordinate is the row number.

## Examples

```
select st_world2rastcoord(rast, 0, '(117.3378,26.9020)'::point) from tb_dem where id = 2;
 st_world2rastcoord 
--------------------
 (53205,32518)

SELECT ST_AsText(ST_world2RastCoord(rast, 0, ST_Rast2WorldCoord(rast, 0, 'POINT(511 0)'::geometry))) 
FROM tb_world2rast;
  st_astext   
--------------
 POINT(511 0)
```

