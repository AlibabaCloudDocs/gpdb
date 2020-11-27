# ST\_MetaData

获得raster对象的元数据，返回JSON格式。

## 语法

```
text ST_MetaData(raster raster_obj);
text ST_MetaData(raster raster_obj,
                text key);
text ST_MetaData(raster raster_obj,
                integer band,
                text key);
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|
|band|波段序号，从0开始。|
|key|需要查询的元数据项名称，如果传入的是`all`，则返回一个包含所有元数据项的JSON。|

## 示例

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

