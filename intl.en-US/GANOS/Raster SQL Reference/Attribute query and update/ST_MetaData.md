# ST\_MetaData

This function returns the metadata of a raster object in JSON format.

## Syntax

```
text ST_MetaData(raster raster_obj);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The raster object.|

## Examples

```
select ST_MetaData(raster_obj) from raster_table;
```

