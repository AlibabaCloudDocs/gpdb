# ST\_BestPyramidLevel

根据视口的世界坐标范围，长和宽来计算最佳的金字塔层级。

## 语法

```
integer ST_BestPyramidLevel(raster rast, Box extent, integer width, integer height );
```

## 参数

|参数名称|描述|
|:---|:-|
|rast|需要转换的raster对象。|
|box|视口的世界空间坐标范围，格式为`((minX,minY),(maxX,maxY))`。|
|width|视口的像素宽度。|
|height|视口的像素高度。|

## 描述

raster对象必须要有完整的空间参考信息（srid值有效）。

## 示例

```
Select ST_BestPyramidLevel(raster_obj, '((128.0, 30.0),(128.5, 30.5))', 800, 600) from raster_table where id = 10;

---------------------
3
```

