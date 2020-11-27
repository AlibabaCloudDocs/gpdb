# ST\_startTime

获得轨迹的起始时间。

## 语法

```
timestamp ST_startTime(trajectory traj) ;
```

## 参数

|参数名称|描述|
|----|--|
|traj|需要获得开始时间的轨迹。|

## 示例

```
Select ST_startTime(traj) From traj_table;
```

