# ST\_SetSrid

设置raster对象的空间参考标识符。空间参考标识符（SRID）的标识以及定义保存在系统表 spatial\_ref\_sys。

## 语法

```
raster ST_SetSrid(raster rast, integer srid);
```

## 参数

|参数名称|描述|
|----|--|
|rast|raster对象。|
|srid|指定的空间参考标识符。|

## 描述

对于存储模式为External的数据，执行该操作时必须确定更新的栅格对象已被空间参考并且更新的SRID能在空间参考系统表中找到，否则更新无效。

## 示例

```
update rast set rast=ST_SetSrid(rast,4326) where id=1;
__________________________________
(1 row)
```

