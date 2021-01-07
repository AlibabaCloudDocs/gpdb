# ST\_DeletePyramid

This function deletes a pyramid for a raster object.

## Syntax

```
raster ST_deletePyramid(raster source);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|source|The raster object.|

## Description

This function deletes the pyramid, resets the metadata, and deletes the chunk data for the raster object.

## Examples

```
DO $$
declare
    rast raster;
begin
    select raster_obj into rast from raster_table where id = 1;
    rast = ST_deletePyramid(rast);
    update raster_table set raster_obj = rast where id = 1;
end;    
$$ LANGUAGE 'plpgsql';
```

