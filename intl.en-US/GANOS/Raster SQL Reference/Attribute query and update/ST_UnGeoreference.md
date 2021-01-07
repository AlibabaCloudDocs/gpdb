# ST\_UnGeoreference

This function deletes the geographic reference information about a raster object.

## Syntax

```
 raster ST_UnGeoreference(raster rast)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|rast|The raster object.|

## Examples

```
update rast set rast=ST_UnGeoreference(rast) where id=1;

__________________________________
(1 row)
```

