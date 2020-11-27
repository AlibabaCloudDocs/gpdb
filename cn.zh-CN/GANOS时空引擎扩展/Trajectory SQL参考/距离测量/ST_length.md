# ST\_length

获取一条轨迹的行程总长度，单位为米。

## 语法

```
float8 ST_length(trajectory traj, integer srid default 0);
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹对象。|
|sird|轨迹点坐标的空间参考值，默认为0（4326）。|

## 描述

根据轨迹srid值计算椭球面长度，单位为米；通常trajectory对象中会有srid值，如果trajectory对象没有srid值，可通过函数参数srid指定，如果sird仍未知，函数将默认srid为4326。

## 示例

```
select st_length(traj) from traj where id = 2;
    st_length     
------------------
 13494.6660605311
(1 row)
```

