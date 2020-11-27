# ST\_subTrajectorySpatial

指定时间段的轨迹几何。

## 语法

```
geometry ST_subTrajectorySpatial(trajectory traj, timestamp starttime, timestamp endtime）;
geometry ST_subTrajectorySpatial(trajectory traj, tsrange range）;
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹对象。|
|starttime|开始时间。|
|endtime|结束时间。|
|range|时间段。|

## 示例

```
Select ST_subTrajectorySpatial(traj, '2010-1-11 02:45:30', '2010-1-11 03:00:00') FROM traj_table;
```

