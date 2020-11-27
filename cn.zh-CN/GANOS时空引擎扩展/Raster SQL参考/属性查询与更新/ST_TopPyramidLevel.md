# ST\_TopPyramidLevel

获得raster对象的金字塔最高层级。

## 语法

```
integer TopPyramidLevel(raster raster_obj);
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|raster对象。|

## 示例

```
select ST_TopPyramidLevel(raster_obj) from raster_table;

__________________________________
6
```

