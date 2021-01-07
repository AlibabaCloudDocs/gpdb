# ST\_SetNoData

This function sets the NoData value for a specified band of a raster object.

## Syntax

```
raster ST_SetNoData(raster rast, integer band_sn, double nodata_value);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|rast|The raster object.|
|band|The sequence number of the band, which starts from 0. A value of -1 indicates all bands.|
|nodata\_value|The NoData value of the band.|

## Examples

```
update rast set rast=ST_SetNoData(rast,0, 999.999);

__________________________________
(1 row)
```

