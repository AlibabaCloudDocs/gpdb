# ST\_MakeBox

获取指定类型在指定时间段内的外包框。

## 语法

```
boxndf ST_MakeBox(geometry geom);
boxndf ST_MakeBox(trajectory traj);
boxndf ST_MakeBox(geometry geom, timestamp ts, timestamp te);
boxndf ST_MakeBox(trajectory traj, timestamp ts, timestamp te);
boxndf ST_MakeBox(timestamp ts, timestamp te);
```

## 参数

|参数名称|描述|
|----|--|
|geom|需要获取的外包框的几何对象。|
|traj|需要获取的外包框的轨迹对象。|
|ts|指定时间段的开始时间。|
|te|指定时间段的结束时间。|

## 描述

外包框即包含某个时空物体的最小多维矩形。

-   对于二维或三维的空间对象，返回其对应维度的外包框。
-   对于指定空间对象和时间段起止时间的调用`ST_MakeBox(geometry geom, timestamp ts, timestamp te)`， 将返回目标对象在目标时间段内的外包框。
-   对于轨迹，若无时间段参数，则返回其整体的外包框；若有时间段参数，则返回在指定时间段内子轨迹的外包框。
-   由于BoxNDF内部由float类型表示，因此得到的Box可能会略大于输入的参数，例如下界比实际值略小或上界比实际值略大。

## 示例

```
WITH geom AS(
    SELECT ('POLYGON((12.7243236691148 4.35238368367118,12.9102992732078 1.49748113937676,12.5926592946053 1.67643963359296' ||
            ',12.0197574747333 3.19258554889152,12.7243236691148 4.35238368367118))')::geometry a
)
SELECT ST_MakeBox(a),ST_MakeBox(a,'2000-01-01 00:00:10'::timestamp, '2000-01-01 02:13:20'::timestamp) from geom;
                           st_makebox                           |                                                      st_makebox                                                       
----------------------------------------------------------------+-----------------------------------------------------------------------------------------------------------------------
 BOX2D(12.0197572708 1.49748110771,12.9102993011 4.35238409042) | BOX2DT(12.0197572708 1.49748110771 2000-01-01 00:00:09.999999,12.9102993011 4.35238409042 2000-01-01 02:13:20.000381)
 
 
With traj AS (
    Select ST_makeTrajectory('STPOINT', 'LINESTRING(0 0, 50 50, 100 100)'::geometry,
                             tsrange('2000-01-01 00:00:00'::timestamp, '2000-01-01 00:01:40'::timestamp),
                             '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120,130,140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120,130,140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120,130,140]}, "acceleration": {"type": "string", "length": 20, "nullable" : true,"value": ["120","130","140"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 11:35:00 2010", "Fri Jan 01 12:35:00 2010", "Fri Jan 01 13:30:00 2010"]}}, "events": [{"2" : "Fri Jan 02 15:00:00 2010"}, {"3" : "Fri Jan 02 15:30:00 2010"}]}') a
)
SELECT  ST_MakeBox(a), ST_MakeBox(a,'1999-12-31 23:00:00'::timestamp, '2000-01-01 00:00:30'::timestamp) from traj;
                         st_makebox                          |                            st_makebox                            
-------------------------------------------------------------+------------------------------------------------------------------
 BOX2DT(0 0 2000-01-01 00:00:00,100 100 2000-01-01 00:01:40) | BOX2DT(0 0 2000-01-01 00:00:00,30 30 2000-01-01 00:00:30.000001)
```

