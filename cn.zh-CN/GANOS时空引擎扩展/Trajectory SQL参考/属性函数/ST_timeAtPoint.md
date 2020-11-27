# ST\_timeAtPoint

获取移动轨迹通过某位置的时间点集合。

## 语法

```
timestamp[] ST_timeAtPoint(trajectory traj, geomery g);
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹对象。|
|g|点位置。|

## 示例

```
Select ST_timeAtPoint(traj, st_geomfromtext('POINT(115 36)')) from traj_table;
     st_timeatpoint      
-------------------------
 {"2010-01-01 15:00:00"}
```

