# ST\_cumulativeDistanceAtTime

从起点出发到指定时间点累计的位移长度。

## 语法

```
float8 ST_cumulativeDistanceAtTime(trajectory traj, timestamp t) ;
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹对象。|
|t|时间点。|

## 示例

```
Select ST_cumulativeDistanceAtTime(traj, '2011-11-1 04:30:00') from traj_table;
```

