# ST\_ScaleX

获得raster对象在空间参考系下X方向的像素宽度。

## 前提条件

raster对象必须已经进行空间参考。

## 语法

```
float8 ST_ScaleX(raster raster_obj)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|

## 示例

```
select st_scalex(rast), st_scaley(rast)
from raster_table;

 st_scalex | st_scaley 
-----------+-----------
       1.3 |       0.3
```

