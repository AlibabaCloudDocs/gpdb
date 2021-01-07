# ST\_SetMetaData

This topic describes the ST\_SetMetaData function, which specifies a metadata item of a raster or band.

## Syntax

```
raster ST_SetMetaData(raster raster_obj,
                      text key,
                      text value);
raster ST_MetaData(raster raster_obj,
                   integer band,
                   text key,
                   text value);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster whose metadata item you want to specify.|
|band|The sequence number of the band whose metadata item you want to specify. Valid values start from 0.|
|key|The name of the metadata item that you want to specify.|
|value|The value of the metadata item.|

## Description

If you specify an empty value \(`''`\) for a metadata item, the metadata item is deleted.

## Examples

```
SELECT ST_MetaData(ST_SetMetaData(rast, 'NETCDF_DIM_time', '12345'), 'NETCDF_DIM_time')
FROM raster_table

st_metadata 
------------
12345


SELECT ST_MetaData(ST_SetMetaData(rast, 0, 'NETCDF_DIM_time', '12345'), 0, 'NETCDF_DIM_time')
FROM raster_table

st_metadata 
------------
12345
```

