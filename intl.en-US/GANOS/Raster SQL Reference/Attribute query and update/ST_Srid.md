# ST\_Srid

This function returns the spatial reference system identifier \(SRID\) of a raster object. The SRID and its definition are stored in the spatial\_ref\_sys table.

## Syntax

```
integer ST_Srid(raster raster_obj);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster object.|

## Examples

```
select ST_Srid(raster_obj) from raster_table where id=1;

__________________________________
4326
```

