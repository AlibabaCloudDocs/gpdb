# ST\_NumBands

获得raster对象的波段数。

## 语法

```
integer ST_NumBands(raster raster_obj);
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|raster对象。|

## 示例

```
select ST_NumBands(raster_obj) from raster_table;

——————————————————————————————————
3
```

