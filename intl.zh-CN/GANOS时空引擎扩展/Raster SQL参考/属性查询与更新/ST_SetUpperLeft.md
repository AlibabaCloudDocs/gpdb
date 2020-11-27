# ST\_SetUpperLeft

设置raster对象在空间参考系下左上角点X、Y值。

## 语法

```
raster ST_SetUpperLeft(raster raster_obj,  float8 upperleftX, float8 upperleftY) 
raster ST_SetUpperLeft(raster raster_obj,  float8 upperleftXY)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|
|upperleftX|空间参考系下左上角点X。|
|upperleftY|空间参考系下左上角点Y。|
|upperleftXY|空间参考系下左上角点X、Y值（X、Y为相同值）。|

## 示例

```
UPDATE raster_table
SET rast = ST_SetUpperleft(rast, 120, 30)
WHERE id = 2;

select st_upperleftx(rast), st_upperlefty(rast)
from raster_table
WHERE id = 2;

 st_upperleftx | st_upperlefty 
---------------+---------------
          120  |            30
```

