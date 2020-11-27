# ST\_SetDateTime

设置栅格对象的起始和终止时间或波段时间。

## 语法

```
raster ST_setDateTime(raster raster_obj,
                      timestamp start,
                      timestamp end);
raster ST_setDateTime(raster raster_obj,
                      integer band,
                      timestamp time);
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|需要计算的raster对象。|
|band|波段序号，取值从0开始。|
|time|波段时间。建议使用`yyyy-MM-dd HH:mm:ss`的格式，例如`2020-08-30 18:00:00`。|
|start|开始时间。建议使用`yyyy-MM-dd HH:mm:ss`的格式。|
|end|结束时间。建议使用`yyyy-MM-dd HH:mm:ss`的格式。|

## 描述

获得栅格对象自定义元数据项的名称列表。

## 示例

```
select ST_DateTime(ST_setDateTime(raster_obj, 0, '2020-01-02'::timestamp), 0)
FROM raster_table

       st_datetime        
--------------------------
 Thu Jan 02 00:00:00 2020
```

