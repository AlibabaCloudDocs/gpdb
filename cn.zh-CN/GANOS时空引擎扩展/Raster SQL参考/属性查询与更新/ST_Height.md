# ST\_Height

获得raster对象的高度。

## 语法

```
integer ST_Height(raster raster_obj);
integer ST_Height(raster raster_obj, integer pyramid)
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|raster对象。|
|pyramid|金字塔层级，从0开始。|

## 示例

```
select ST_Height(raster_obj) from raster_table;
```

