# ST\_NoData

This function returns the predefined NoData value for a band of a raster object. If the NoData value is not defined, the function returns null.

## Syntax

```
 float8 ST_NoData(raster raster_obj, integer band_sn)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The raster object.|
|band\_sn|The sequence number of the band, which starts from 0.|

## Examples

```
select ST_NoData(raster_obj, 0) from raster_table where id=1;

__________________________________
0.000
```

