# ST\_Rast2WorldCoord

由像元坐标及像元所在金字塔层级，根据仿射变换公式计算世界坐标。

## 语法

```
point ST_Rast2WorldCoord(raster raster_obj, integer pyramidLevel,  integer row, integer column);
geometry ST_Rast2WorldCoord(raster raster_obj, integer pyramidLevel,  geometry geom);
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|目标raster对象|
|pyramidLevel|金字塔层级|
|row|行号|
|column|列号|
|geom|需要转换的几何对象，横坐标x值表示象元的列号，纵坐标y值表示象元的行号|

## 描述

raster对象必须要有完整的空间参考信息。

## 示例

```
SELECT ST_rast2WorldCoord(raster_obj, 0, 3, 4) FROM raster_table;
 st_rast2worldcoord 
--------------------
 (440960,3751140)
 
 SELECT ST_AsText(ST_rast2WorldCoord(raster_obj, 0, 'POINT(4 3)'::geometry)) FROM raster_table;
       st_astext       
-----------------------
 POINT(440960 3751140)
```

