# ST\_MosaicFrom

This function performs a mosaic operation to combine multiple raster objects into a new raster object.

## Syntax

```
raster ST_MosaicFrom(raster source[],  cstring chunkTableName);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|source|The name of the raster objects that you want to combine.|
|chunkTableName|The name of the chunk table for storing the new raster object. The name must comply with the table naming conventions of ApsaraDB RDS for PostgreSQL.|

## Description

This function combines multiple raster objects into a new raster object.

All of the raster objects that you want to combine must meet the following requirements:

-   All of the raster objects have the same number of bands.
-   All of the raster objects are geographically referenced, or no raster objects are geographically referenced. If all of the raster objects are geographically referenced, world coordinates \(geographic coordinates\) are used for the mosaic operation.
-   Raster objects can have different pixel types. If world coordinates are used for the mosaic operation, they must have the same spatial reference system identifier \(SRID\) and affine parameters.

## Examples

```
DO $$
declare
    rasts raster[];
    rast_mosaic raster;
begin
    rasts = Array(select raster_obj from raster_table where id < 5 ORDER by id);
    rast_mosaic = ST_MosaicFrom(rasts, 'chunk_table_mosaic');
    Insert Into raster_table Values(10, rast_mosaic); 
end;    
$$ LANGUAGE 'plpgsql';
```

