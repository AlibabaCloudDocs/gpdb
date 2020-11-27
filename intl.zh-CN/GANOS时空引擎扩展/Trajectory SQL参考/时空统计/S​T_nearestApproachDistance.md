# S​T\_nearestApproachDistance

指定时间段的轨迹1和轨迹2中找到最邻近距离。

## 语法

```
float8 S​T_nearestApproachDistance(trajectory traj,  trajectory traj2);
float8 S​T_nearestApproachDistance(trajectory traj,  trajectory traj2, tsrange range);
float8 S​T_nearestApproachDistance(trajectory traj,  trajectory traj2, timestamp t1, timestamp t2);
```

## 参数

|参数名称|描述|
|----|--|
|traj1|轨迹对象1。|
|traj2|轨迹对象2。|
|t1|开始时间。|
|t2|结束时间。|
|range|时间段。|

## 示例

```
Select ST_nearestApproachDistance((Select traj from traj_table where id=1), (Select traj from traj_table where id=2), '2010-1-1 13:00:00', '2010-1-1 14:00:00');
```

