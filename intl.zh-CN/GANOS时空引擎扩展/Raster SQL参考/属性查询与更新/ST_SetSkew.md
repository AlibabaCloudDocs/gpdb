# ST\_SetSkew

设置raster对象X、Y方向的旋转。

## 语法

```
raster ST_SetSkew(raster raster_obj,  float8 skewX, float8 skewY) 
raster ST_SetSkew(raster raster_obj,  float8 skewXY)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|
|skewX|X方向的旋转。|
|skewY|Y方向的旋转。|
|skewXY|X、Y方向的旋转（X、Y为相同值）。|

## 示例

```
UPDATE raster_table
SET rast = ST_SetSkew(rast, 0.3, 0.6)
WHERE id = 2;

select st_skewx(rast), st_skewy(rast)
from raster_table
WHERE id = 2;

 st_skewx | st_skewy 
----------+-----------
      0.3 |      0.6
```

