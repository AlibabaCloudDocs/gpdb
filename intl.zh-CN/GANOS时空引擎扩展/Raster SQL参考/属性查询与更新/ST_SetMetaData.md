# ST\_SetMetaData

设置栅格对象或波段的元数据项。

## 语法

```
raster ST_SetMetaData(raster raster_obj,
                      text key,
                      text value);
raster ST_MetaData(raster raster_obj,
                   integer band,
                   text key,
                   text value);
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|需要计算的raster对象。|
|band|波段序号，取值从0开始。|
|key|需要设置的元数据项名称。|
|value|元数据值。|

## 描述

如果传入的元数据值为空值（`''`），会删除该元数据项。

## 示例

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

