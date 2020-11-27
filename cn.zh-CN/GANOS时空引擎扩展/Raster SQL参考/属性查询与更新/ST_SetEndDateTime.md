# ST\_SetEndDateTime

设置栅格对象的结束时间。

## 语法

```
raster ST_SetEndDateTime(raster raster_obj, timestamp time);
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|需要计算的raster对象。|
|time|需要设置的时间。建议使用`yyyy-MM-dd HH:mm:ss`的格式，例如`2020-08-30 18:00:00`。|

## 示例

```
SELECT ST_endDateTime(ST_setEndDateTime(raster_obj, '2020-01-01'))
FROM raster_table;

    st_enddatetime     
-------------------------
Wed Jan 01 00:00:00 2020                                                                                
```

