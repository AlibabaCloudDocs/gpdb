# S​T\_nearestApproachDistance

指定时间段的轨迹中找到与给定几何对象最近距离。

## 语法

```
float8 S​T_nearestApproachDistance(trajectory traj, tsrange range, geometry g);
float8 S​T_nearestApproachDistance(trajectory traj, timestamp t1, timestamp t2, geometry g);
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹对象。|
|t1|开始时间。|
|t2|结束时间。|
|range|时间段。|
|g|几何对象。|

## 示例

```
Select ST_nearestApproachDistance(traj, '2010-1-1 13:00:00', '2010-1-1 14:00:00', 'LINESTRING(0 0, 5 5, 9 9)'::geometry) from  traj_table;
```

