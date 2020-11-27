# ST\_accelerationAtTime

获取指定时间位置的加速度属性。

## 语法

```
float8 ST_accelerationAtTime (trajectory traj, timestamp t);
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹对象。|
|t|指定的时间点。|

## 示例

```
Select ST_accelerationAtTime(traj,'2010-1-11 23:40:00') From traj_table;
```

