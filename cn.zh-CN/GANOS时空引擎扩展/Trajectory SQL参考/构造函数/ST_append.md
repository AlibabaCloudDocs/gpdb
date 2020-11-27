# ST\_append

向轨迹中追加轨迹点或子轨迹。

## 语法

```
trajectory ST_append(trajectory traj, geometry spatial, timestamp[] timespan, text str_attrs_json) ;
trajectory ST_append(trajectory traj, trajectroy tail) ; 
```

## 参数

|参数名称|描述|
|----|--|
|traj|原轨迹。|
|spatial|追加的轨迹空间对象。|
|timespan|追加的轨迹的时间数组，可为时间序列。|
|str\_attrs\_json|追加轨迹的属性信息，参见[ST\_makeTrajectory](/cn.zh-CN/时空数据库/Trajectory SQL参考/构造函数/ST_makeTrajectory.md)。|
|tail|追加的轨迹。|

## 示例

```
With traj AS (  Select ST_makeTrajectory('STPOINT', 'LINESTRING(1 1, 6 6, 9 8)'::geometry, '[2010-01-01 11:30, 2010-01-01 15:00)'::tsrange,  '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120, 130, 140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120, 130, 140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120, 130, 140]}, "acceleration": {"type": "string", "length": 20, "nullable" : true,"value": ["120", "130", "140"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 14:30:00 2010", "Fri Jan 01 15:00:00 2010", "Fri Jan 01 15:30:00 2010"]}}, "events": [{"1" : "Fri Jan 01 14:30:00 2010"}, {"2" : "Fri Jan 01 15:00:00 2010"}, {"3" : "Fri Jan 01 15:30:00 2010"}]}') a,  ST_makeTrajectory('STPOINT', 'LINESTRING(7 7, 3 4, 1 5)'::geometry, '[2010-01-02 15:30, 2010-01-02 18:00)'::tsrange,  '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [121, 131, 141]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [121, 131, 141]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [121, 131, 141]}, "acceleration": {"type": "string", "length": 20, "nullable" : true,"value": ["121", "131", "141"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 02 14:30:00 2010", "Fri Jan 02 15:00:00 2010", "Fri Jan 02 15:30:00 2010"]}}, "events": [{"1" : "Fri Jan 02 14:30:00 2010"}, {"2" : "Fri Jan 02 15:00:00 2010"}, {"3" : "Fri Jan 02 15:30:00 2010"}]}') b)Select ST_Append(a, b) from traj;
                st_append                                                                                                                                                                                                                                                        
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 {"trajectory":{"version":1,"type":"STPOINT","leafcount":6,"start_time":"2010-01-01 11:30:00","end_time":"2010-01-02 18:00:00","spatial":"LINESTRING(1 1,6 6,9 8,7 7,3 4,1 5)","timeline":["2010-01-01 11:30:00","2010-01-01 13:15:00","2010-01-01 15:00:00","2010-01-02 15:30:00","2010-01-02 16:45:00","2010-01-02 18:00:00"],"attributes":{"leafcount":6,"velocity":{"type":"integer","length":2,"nullable":true,"value":[120,130,140,121,131,141]},"accuracy":{"type":"float","length":4,"nullable":false,"value":[120.0,130.0,140.0,121.0,131.0,141.0]},"bearing":{"type":"float","length":8,"nullable":false,"value":[120.0,130.0,140.0,121.0,131.0,141.0]},"acceleration":{"type":"string","length":20,"nullable":true,"value":["120","130","140","121","131","141"]},"active":{"type":"timestamp","length":8,"nullable":false,"value":["2010-01-01 14:30:00","2010-01-01 15:00:00","2010-01-01 15:30:00","2010-01-02 14:30:00","2010-01-02 15:00:00","2010-01-02 15:30:00"]}},"events":[{"1":"2010-01-01 14:30:00"},{"2":"2010-01-01 15:00:00"},{"3":"2010-01-01 15:30:00"},{"1":"2010-01-02 14:30:00"},{"2":"2010-01-02 15:00:00"},{"3":"2010-01-02 15:30:00"}]}}
(1 row)
```

