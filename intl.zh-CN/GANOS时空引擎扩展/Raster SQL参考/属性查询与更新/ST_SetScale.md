# ST\_SetScale

设置raster对象在空间参考系下X、Y方向的像素宽度。

## 语法

```
raster ST_SetScale(raster raster_obj,  float8 scaleX, float8 scaleY) 
raster ST_SetScale(raster raster_obj,  float8 scaleXY)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|
|scaleX|空间参考系下X方向像素宽。|
|scaleY|空间参考系下Y方向像素宽。|
|scaleXY|空间参考系下X、Y方向像素宽（X、Y为相同值）。|

## 示例

```
UPDATE raster_table
SET rast = ST_SetScale(rast, 1.3, 0.6)
WHERE id = 2;

select st_scalex(rast), st_scaley(rast)
from raster_table
WHERE id = 2;

 st_scalex | st_scaley 
-----------+-----------
       1.3 |       0.6
```

