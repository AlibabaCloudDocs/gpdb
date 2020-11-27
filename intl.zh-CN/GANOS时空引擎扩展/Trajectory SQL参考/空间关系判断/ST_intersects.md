# ST\_intersects

指定时间区间的轨迹段和几何图形空间是否相交。

## 语法

```
boolean ST_intersects(trajectory traj, tsrange range, geometry g);
boolean ST_intersects(trajectory traj, timestamp t1, timestamp t2, geometry g);
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
Select ST_intersects(traj, '2010-1-1 13:00:00', '2010-1-1 14:00:00', 'LINESTRING(0 0, 5 5, 9 9)'::geometry) from traj_table;
```

