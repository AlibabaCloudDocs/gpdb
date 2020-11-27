# ST\_MetaItems

获得栅格对象自定义元数据项的名称列表。

## 语法

```
text[] ST_metaItems(raster raster_obj);
text[] ST_metaItems(raster raster_obj,
                    integer band);
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|需要计算的raster对象。|
|band|波段序号，取值从0开始。|

## 示例

```
-- meta data items of a raster
SELECT ST_MetaItems(raster_obj) 
FROM raster_table;
                                                                       st_metaitems                                                                                                                                    
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 {latitude#long_name,latitude#units,longitude#long_name,longitude#units,NC_GLOBAL#Conventions,NC_GLOBAL#history,NETCDF_DIM_EXTRA,NETCDF_DIM_time_DEF,NETCDF_DIM_time_VALUES}
  
-- meta data items of a band
SELECT ST_MetaItems(raster_obj, 0) 
FROM raster_table;

                                       st_metaitems                                        
-------------------------------------------------------------------------------------------
{add_offset,long_name,missing_value,NETCDF_DIM_time,NETCDF_VARNAME,scale_factor,units,_FillValue}                                                                                  
```

