# ST\_timeToDistance

输出时间到欧氏距离的函数，以折线输出，横坐标为时间，纵坐标为距离。

## 语法

```
geometry ST_timeToDistance(trajectory traj);
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹对象。|

## 示例

```
Select ST_timeToDistance(traj) from traj_table;
```

