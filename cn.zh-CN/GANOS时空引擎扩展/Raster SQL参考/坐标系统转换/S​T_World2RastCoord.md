# S​T\_World2RastCoord

由世界坐标及像元所在金字塔层级，根据逆仿射变换公式计算像元坐标。

## 语法

```
point ST_World2RastCoord(raster raster_obj， integer pyramidLevel, point coord);
geometry ST_World2RastCoord(raster raster_obj， integer pyramidLevel, geometry geom);
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|需要转换的raster对象。|
|pyramidLevel|需要转换的金字塔层级。|
|coord|需要转换的世界空间坐标。|
|geom|需要转换的几何对象。|

## 描述

raster对象必须要有完整的空间参考信息。

返回的几何对象，横坐标x值表示象元的列号，纵坐标y值表示象元的行号。

## 示例

```
select st_world2rastcoord(rast, 0, '(117.3378,26.9020)'::point) from tb_dem where id = 2;
 st_world2rastcoord 
--------------------
 (53205,32518)

SELECT ST_AsText(ST_world2RastCoord(rast, 0, ST_Rast2WorldCoord(rast, 0, 'POINT(511 0)'::geometry))) 
FROM tb_world2rast;
  st_astext   
--------------
 POINT(511 0)
```

