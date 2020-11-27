# ST\_timeAtDistance

从起始点移动指定距离后所在的时间点。

## 语法

```
timestamp[] ST_timeAtDistance(trajectory traj, float8 d);
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹对象。|
|d|距离。|

返回的是 timestamp的数组，有可能存在多个点到起始点的距离一致。

## 示例

```
Select ST_timeAtDistance(traj, 100) from traj_table;
```

