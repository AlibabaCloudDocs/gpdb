# ST\_SetName

This function sets the name of a raster object.

## Syntax

```
raster ST_SetName(raster rast, cstring name);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|rast|The raster object.|
|name|The name of the raster object.|

## Examples

```
update rat set rast = ST_SetName(rast,'image2') where id = 2;
---------------------------------------------------------------
(1 row)
```

