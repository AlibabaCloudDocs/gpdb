# ST\_Extent

获得raster对象的坐标范围，返回PostgreSQL的BOX对象，格式为`'((maxX,maxY),(minX,minY))'`。

## 语法

```
BOX ST_Extent(raster raster_obj,CoorSpatialOption csOption = 'WorldFirst')
BOX ST_Extent(raster raster_obj, integer pyramid, CoorSpatialOption csOption = 'WorldFirst')
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|raster对象。|
|csOption|坐标空间选项枚举值之一。|
|pyramid|金字塔层级，从0开始。|

## 描述

csOption为坐标空间选项，取值如下：

-   Raster：影像坐标空间，返回像元坐标。
-   World：世界坐标空间，返回世界坐标。
-   WorldFirst：世界坐标空间优先，即如果已进行地理参考，则返回世界坐标，如果未进行地理参考，则返回像元坐标。

## 示例

```
select ST_Extent(raster_obj, 'Raster'::CoorSpatialOption) from raster_table;

__________________________________
((255, 255), (0, 0))
```

