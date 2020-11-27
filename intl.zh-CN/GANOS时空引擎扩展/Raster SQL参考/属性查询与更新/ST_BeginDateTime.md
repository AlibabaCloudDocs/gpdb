# ST\_BeginDateTime

获得栅格对象的开始时间。

## 语法

```
timestamp ST_BeginDateTime(raster raster_obj);
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|需要计算的raster对象。|

## 示例

```
SELECT ST_beginDateTime(raster_obj)
FROM raster_table;

    st_begindatetime     
-------------------------
Wed Jan 01 00:00:00 2020 
```

