# ST\_Rast2WorldCoord

This function calculates the world coordinates of a cell by using an affine transformation formula based on the pixel coordinates and pyramid level of the cell.

## Syntax

```
point ST_Rast2WorldCoord(raster raster_obj, integer pyramidLevel, integer row, integer column);
geometry ST_Rast2WorldCoord(raster raster_obj, integer pyramidLevel, geometry geom);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster object.|
|pyramidLevel|The pyramid level.|
|row|The row number of the cell.|
|column|The column number of the cell.|
|geom|The geometry must be converted. The x coordinate of the geometry represents the column number in the raster and the y coordinate of the geometry represents the row number .|

## Description

The raster object must have a valid spatial reference system identifier \(SRID\).

## Examples

```
SELECT ST_rast2WorldCoord(raster_obj, 0, 3, 4) FROM raster_table;
 st_rast2worldcoord 
--------------------
 (440960,3751140)
 
 SELECT ST_AsText(ST_rast2WorldCoord(raster_obj, 0, 'POINT(4 3)'::geometry)) FROM raster_table;
       st_astext       
-----------------------
 POINT(440960 3751140)
```

