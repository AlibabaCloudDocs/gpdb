# ST\_addEvent

给轨迹增加一个事件。

## 语法

```
trajectory ST_addEvent(trajectory traj, integer event_type, timestamp event_time) ;
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹对象。|
|event\_type|事件类型。|
|event\_time|事件时间戳。|

## 描述

事件类型由用户预先定义好，如 ‘1000’ 表示开锁， ‘2000’表示关锁等。

## 示例

```
With traj AS (
  select '{"trajectory":{"version":1,"type":"STPOINT","leafcount":2,"start_time":"2010-01-01 11:30:00","end_time":"2010-01-01 12:30:00","spatial":"SRID=4326;LINESTRING(1 1,3 5)","timeline":["2010-01-01 11:30:00","2010-01-01 12:30:00"],"attributes":{"leafcount":2,"velocity":{"type":"integer","length":4,"nullable":true,"value":[1,null]},"speed":{"type":"float","length":8,"nullable":true,"value":[null,1.0]},"angel":{"type":"string","length":64,"nullable":true,"value":["test",null]}, "tngel2":{"type":"timestamp","length":8,"nullable":true,"value":["2010-01-01 12:30:00",null]},"bearing":{"type":"bool","length":1,"nullable":true,"value":[null,true]}}}}'::trajectory a)
Select st_addevent(a, 1, '2010-01-01 11:30:00' ) from traj;
                                                                                                                                                                                                                                                                                                                                                st_addevent                                                                                                                                                                                                                                                                                                                                                 
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 {"trajectory":{"version":1,"type":"STPOINT","leafcount":2,"start_time":"2010-01-01 11:30:00","end_time":"2010-01-01 12:30:00","spatial":"SRID=4326;LINESTRING(1 1,3 5)","timeline":["2010-01-01 11:30:00","2010-01-01 12:30:00"],"attributes":{"leafcount":2,"velocity":{"type":"integer","length":4,"nullable":true,"value":[1,null]},"speed":{"type":"float","length":8,"nullable":true,"value":[null,1.0]},"angel":{"type":"string","length":64,"nullable":true,"value":["test",null]},"tngel2":{"type":"timestamp","length":8,"nullable":true,"value":["2010-01-01 12:30:00",null]},"bearing":{"type":"bool","length":1,"nullable":true,"value":[null,true]}},"events":[{"1":"2010-01-01 11:30:00"}]}}
(1 row)
```

