# ST\_SetMD5Sum

设置raster对象的MD5字符串。

## 语法

```
text ST_SetMD5Sum(raster raster_obj, text md5sum)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|
|md5sum|MD5字符串。|

## 描述

将MD5字符串记录到raster对象中。MD5字符串长度必须为32位，并且只能由数字和小写字母组成。

GUC ganos.raster.calculate\_md5：指定是否在raster对象入库时计算md5sum并保存到元数据中。有关变量信息请参见[ganos.raster.calculate\_md5](/intl.zh-CN/GANOS时空引擎扩展/Raster SQL参考/变量/ganos.raster.calculate_md5.md)。

## 示例

```
SELECT ST_MD5SUM(ST_SetMD5Sum(rast,'21f41fd983d3139c75b04bff2b7bf5c9'))
FROM raster_table
WHERE id = 1;

              st_md5sum
-----------------------------------
 21f41fd983d3139c75b04bff2b7bf5c9
```

