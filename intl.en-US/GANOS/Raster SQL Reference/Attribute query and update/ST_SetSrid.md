# ST\_SetSrid

This function sets the spatial reference system identifier \(SRID\) of a raster object. The SRID and its definition are stored in the spatial\_ref\_sys table.

## Syntax

```
raster ST_SetSrid(raster rast, integer srid);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|rast|The raster object.|
|srid|The SRID of the raster object.|

## Description

If the raster object is stored in external mode, you must make sure that the raster object is geographically referenced and the specified SRID can be found in the spatial\_ref\_sys table. Otherwise, the information about the raster object cannot be updated.

## Examples

```
update rast set rast=ST_SetSrid(rast,4326) where id=1;
__________________________________
(1 row)
```

