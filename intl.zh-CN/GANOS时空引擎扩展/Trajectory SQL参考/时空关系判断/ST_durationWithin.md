# ST\_durationWithin

指定时间区间的轨迹段1和轨迹段2经过某点（空间相交点）的时间相差是否在指定时间区间内。

## 语法

```
boolean ST_durationWithin(trajectory traj1, trajectory traj2, tsrange range, interval i );
boolean ST_durationWithin(trajectory traj1, trajectory traj2, timestamp t1, timestamp t2, interval i);
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹对象。|
|t1|开始时间。|
|t2|结束时间。|
|range|时间段。|
|i|时间间隔。|

## 描述

如果轨迹多次经过相同的点，任意一个时间符合要求就认为符合要求。

## 示例

```
Select ST_durationWithin((Select traj from traj_table where id=1), (Select traj from traj_table where id=2), '2010-1-1 13:00:00', '2010-1-1 14:00:00', INTERVAL '30s');
```

