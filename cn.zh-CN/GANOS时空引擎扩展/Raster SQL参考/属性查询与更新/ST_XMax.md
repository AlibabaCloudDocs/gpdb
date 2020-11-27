# ST\_XMax

获得raster对象在X方向的最大值。

## 语法

```
float8 ST_XMax(raster raster_obj)
float8 ST_XMax(raster raster_obj, integer pyramid)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|
|pyramid|金字塔层级，从0开始。|

## 描述

如果raster对象已经进行地理参考，返回的是X坐标的最大值，否则返回raster对象宽度-1。

## 示例

```
select ST_XMax(rast)
from raster_table;

 st_xmax  
----------
      121 
```

