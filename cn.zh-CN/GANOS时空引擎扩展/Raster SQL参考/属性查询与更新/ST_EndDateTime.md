# ST\_EndDateTime

获得栅格对象的结束时间。

## 语法

```
timestamp ST_EndDateTime(raster raster_obj);
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|需要计算的raster对象。|

## 示例

```
SELECT ST_endDateTime(raster_obj)
FROM raster_table;

     st_enddatetime      
-------------------------
Thu Jan 02 00:00:00 2020 
```

