# ST\_duration

获得该轨迹的持续时间。

## 语法

```
interval ST_uration(trajectory traj);
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹数据。|

## 示例

```
select st_duration(traj) from traj where id = 2;
 st_duration 
-------------
 00:42:10
(1 row)
```

