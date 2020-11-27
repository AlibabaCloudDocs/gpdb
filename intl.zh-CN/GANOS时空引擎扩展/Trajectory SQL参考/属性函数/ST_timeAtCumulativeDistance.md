# ST\_timeAtCumulativeDistance

从起点出发位移到指定长度时所处的时间点。

## 语法

```
timestamp ST_timeAtCumulativeDistance(trajectory traj, float d) ;
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹对象。|
|d|累计长度。|

## 示例

```
Select ST_timeAtCumulativeDistance(traj, 100.0) from traj_table;
```

