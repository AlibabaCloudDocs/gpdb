# ST\_MD5Sum

返回一个raster对象的MD5字符串。

## 语法

```
text ST_MD5Sum(raster raster_obj)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|

## 描述

先试图从元数据中获得MD5字符串，如果元数据中不存在，数据内部raster对象返回NULL，然后OSS存储的raster对象再根据OSS路径进行计算。

GUC ganos.raster.calculate\_md5：指定是否在raster对象入库时计算md5sum并保存到元数据中，默认值是false。有关变量信息请参见[ganos.raster.calculate\_md5](/intl.zh-CN/GANOS时空引擎扩展/Raster SQL参考/变量/ganos.raster.calculate_md5.md)。

GUC ganos.raster.md5sum\_chunk\_size：指定计算md5时每次读入的缓存的大小，默认值是10MB。有关变量信息请参见[ganos.raster.md5sum\_chunk\_size](/intl.zh-CN/GANOS时空引擎扩展/Raster SQL参考/变量/ganos.raster.md5sum_chunk_size.md)。

## 示例

```
SELECT ST_MD5SUM(rast)
FROM raster_table
WHERE id = 1;

              st_md5sum
-----------------------------------
 21f41fd983d3139c75b04bff2b7bf5c9
```

