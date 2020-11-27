# ST\_subTrajectory

根据时间段截取子段。

## 语法

```
trajectory ST_subTrajectory(trajectory traj, timestamp starttime, timestamp endtime）; 
trajectory ST_subTrajectory(trajectory traj, tsrange range);
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
Select ST_subTrajectory(traj, '2010-1-11 02:45:30', '2010-1-11 03:00:00') FROM traj_table;
```

