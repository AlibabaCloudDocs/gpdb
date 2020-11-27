# ST\_pointAtTime

获取指定时间位置的几何对象。

## 语法

```
geometry ST_pointAtTime(trajectory traj, timestamp t);
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹对象。|
|t|指定的时间点。|

返回点对象。

## 示例

```
Select ST_pointAtTime(traj, '2010-1-11 23:40:00') from traj_table;
```

