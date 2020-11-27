# ST\_UpperLeftY

获得raster对象在空间参考系下左上角点Y值。

## 前提条件

raster对象必须已经进行空间参考。

## 语法

```
float8 ST_UpperLeftY(raster raster_obj)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|

## 示例

```
select st_upperleftx(rast), st_upperlefty(rast)
from raster_table;

 st_upperleftx | st_upperlefty 
---------------+---------------
        440720 |       3751320
```

