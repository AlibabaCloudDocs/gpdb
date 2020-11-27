# ST\_mdistance

计算轨迹对象中所有相同的时间点的欧几里得距离。

## 语法

```
float[] ST_mdistance(trajectory traj1, trajectory traj2);
```

## 参数

|参数名称|描述|
|----|--|
|traj1|轨迹对象1。|
|traj2|轨迹对象2。|

## 描述

未经过标准化处理。

## 示例

```
Select ST_mDistance((Select traj from traj_table where id=1), (Select traj from traj_table where id=2));
```

