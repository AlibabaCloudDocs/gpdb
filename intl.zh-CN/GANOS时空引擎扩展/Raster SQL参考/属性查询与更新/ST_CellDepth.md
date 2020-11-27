# ST\_CellDepth

获得raster对象的像素深度。深度值可以为以下值之一：0，1，2，4，8，16，32，64。其中，0为像素深度未知。

## 语法

```
integer ST_CellDepth(raster raster_obj);
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|raster对象。|

## 示例

```
select ST_CellDepth(raster_obj) from raster_table;

__________________________________
8
```

