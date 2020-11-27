# ST\_intersection

给定时间段的轨迹和给定的几何对象执行相交处理，返回相交处理后的轨迹对象。

## 语法

```
trajectory[] ST_intersection(trajectory traj, tsrange range, geometry g);
trajectory[] ST_intersection(trajectory traj, timestamp t1, timestamp t2, geometry g);
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹对象。|
|t1|开始时间。|
|t2|结束时间。|
|range|时间段。|
|g|几何对象。|

## 描述

如果轨迹几何对象和几何对象有多个交点，则返回多个子轨迹。

## 示例

```
Select ST_intersection(traj, '2010-1-1 13:00:00', '2010-1-1 14:00:00', 'LINESTRING(0 0, 5 5, 9 9)'::geometry) from  traj_table;
```

