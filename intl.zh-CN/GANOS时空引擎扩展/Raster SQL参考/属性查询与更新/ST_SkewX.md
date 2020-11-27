# ST\_SkewX

获得raster对象在X方向的旋转。

## 前提条件

raster对象必须已经进行空间参考。

## 语法

```
float8 ST_SkewX(raster raster_obj)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|

## 示例

```
select st_skewx(rast), st_skewy(rast)
from raster_table;

 st_skewx | st_skewy 
----------+-----------
      0.3 |      0.2
```

