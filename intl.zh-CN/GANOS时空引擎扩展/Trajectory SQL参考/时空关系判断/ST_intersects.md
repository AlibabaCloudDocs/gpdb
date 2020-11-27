# ST\_intersects

指定时间区间的轨迹段1和轨迹段2是否相交。

## 语法

```
boolean ST_intersects(trajectory traj1, trajectory traj2);
boolean ST_intersects(trajectory traj1, trajectory traj2, tsrange range);
boolean ST_intersects(trajectory traj1, trajectory traj2, timestamp t1, timestamp t2);
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹对象。|
|t1|开始时间。|
|t2|结束时间。|
|range|时间段。|

## 示例

```
Select ST_intersects((Select traj from traj_table where id=1), (Select traj from traj_table where id=2), '2010-1-1 13:00:00', '2010-1-1 14:00:00');
```

