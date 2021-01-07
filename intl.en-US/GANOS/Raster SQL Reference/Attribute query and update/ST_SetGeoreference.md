# ST\_SetGeoreference

This function sets the geographic reference information about a raster object.

## Syntax

```
raster ST_SetGeoreference(raster rast, integer srid, integer aop, double A, double B, double C, double D, double E, double F)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|rast|The raster object.|
|srid|The spatial reference system identifier \(SRID\) of the raster object.|
|aop|The spatial reference point. Valid values: -   1: the center cell
-   2: the upper-left cell |
|A, B, C, D, E, and F|The six parameters of an affine transformation. -   x = A × Column number + B × Row number + C
-   y = D × Column number + E × Row number + F |

## Description

If the raster object is stored in external mode, you need to ensure that the raster object is geographically referenced and the specified SRID can be found in the spatial\_ref\_sys table. Otherwise, the information about the raster object cannot be updated.

## Examples

```
update rast set rast=ST_SetGeoreference(rast,4326,1,8.4163,0,124,0,-8.4163,36.2) where id=1;

__________________________________
(1 row)
```

