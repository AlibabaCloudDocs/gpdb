# ST\_euclideanDistance

计算两个轨迹对象之间的欧几里得距离。

## 语法

```
float ST_euclideanDistance(trajectory traj1, trajectory traj2);
```

## 参数

|参数名称|描述|
|----|--|
|traj1|轨迹对象1。|
|traj2|轨迹对象2。|

## 描述

距离已经进行了标准化处理。

## 示例

```
Select ST_euclideanDistance((Select traj from traj_table where id=1), (Select traj from traj_table where id=2));
```

