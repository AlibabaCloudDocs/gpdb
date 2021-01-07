# ST\_Georeference

This function returns the geographic reference information about a raster object. Affine parameters in the text format of "A,B,C,D,E,F" are returned.

## Syntax

```
text ST_Georeference(raster raster_obj)
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster object.|

## Examples

```
select ST_Georeference(raster_obj) from raster_table where id=1;

__________________________________
2.500000000000000,0.000000000000000,38604686.750000000000000,0.000000000000000,-2.500000000000000,4573895.750000000000000
```

