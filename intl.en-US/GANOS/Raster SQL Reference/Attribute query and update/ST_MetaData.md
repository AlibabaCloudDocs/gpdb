# ST\_MetaData

This function returns the metadata of a raster object in JSON format.

## Syntax

```
text ST_MetaData(raster raster_obj);
text ST_MetaData(raster raster_obj,
                text key);
text ST_MetaData(raster raster_obj,
                integer band,
                text key);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The raster object.|
|band|The sequence number of the band, which starts from 0.|
|key|The name of the metadata item that you want to query. If you set this parameter to `all`, all metadata items are retutrned in JSON format.|

## Examples

```
select ST_MetaData(raster_obj) from raster_table;

-- meta data with a name
SELECT ST_MetaData(raster_obj, 'swh#scale_factor')
FROM raster_table;

      st_metadata      
-----------------------
 0.0001488117874873806
 
-- all meta data
SELECT ST_MetaData(raster_obj, 'all')
FROM raster_table;

      st_metadata      
-----------------------
{"AREA_OR_POINT":"Area"}

-- meta data with a name of a band
SELECT ST_MetaData(raster_obj, 0, 'NETCDF_DIM_time')
FROM raster_table;

 st_metadata 
-------------
 1043112     
 
 -- all meta data of a band
SELECT ST_MetaData(raster_obj, 'all')
FROM raster_table;

                                                              st_metadata                                                               
----------------------------------------------------------------------------------------------------------------------------------------
{"add_offset":"4.907141431495487","long_name":"Significant height of combined wind waves and swell","missing_value":"-32767","NETCDF_DI.
M_time":"1043112","NETCDF_VARNAME":"swh","scale_factor":"0.0001488117874873806","units":"m","_FillValue":"-32767"}
```

