# ST\_eventTimes

获得轨迹的所有事件时间。

## 语法

```
timestamp[] ST_eventTimes(trajectory traj) ;
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹对象。|

## 示例

```
With traj AS (
  select '{"trajectory":{"version":1,"type":"STPOINT","leafcount":2,"start_time":"2010-01-01 11:30:00","end_time":"2010-01-01 12:30:00","spatial":"SRID=4326;LINESTRING(1 1,3 5)","timeline":["2010-01-01 11:30:00","2010-01-01 12:30:00"],"attributes":{"leafcount":2,"velocity":{"type":"integer","length":4,"nullable":true,"value":[1,null]},"speed":{"type":"float","length":8,"nullable":true,"value":[null,1.0]},"angel":{"type":"string","length":64,"nullable":true,"value":["test",null]}, "tngel2":{"type":"timestamp","length":8,"nullable":true,"value":["2010-01-01 12:30:00",null]},"bearing":{"type":"bool","length":1,"nullable":true,"value":[null,true]}},"events":[{"1":"Fri Jan 01 14:30:00 2010"},{"2":"Fri Jan 01 14:30:00 2010"}]}}'::trajectory a)
Select st_eventTimes(a ) from traj;
                 st_eventtimes                 
-----------------------------------------------
 {"2010-01-01 14:30:00","2010-01-01 14:30:00"}
(1 row)
```

