# ST\_SetGeoreference

设置raster对象的地理参考信息。

## 语法

```
raster ST_SetGeoreference(raster rast, integer srid, integer aop, double A, double B, double C, double D, double E, double F)
```

## 参数

|参数名称|描述|
|----|--|
|rast|raster对象。|
|srid|指定的空间参考标识符。|
|aop|AOP为空间参考点，取像元的中心或左上⻆： -   Center=1
-   Upleft =2 |
|A~F|A~F为仿射变换的六个参数： -   x = A\*col + B\*row + C
-   y = D\*col + E\*row + F |

## 描述

对于存储模式为External的数据，执行该操作时必须确定更新的栅格对象已被空间参考并且更新的SRID能在空间参考系统表中找到，否则更新无效。

## 示例

```
update rast set rast=ST_SetGeoreference(rast,4326,1,8.4163,0,124,0,-8.4163,36.2) where id=1;

__________________________________
(1 row)
```

