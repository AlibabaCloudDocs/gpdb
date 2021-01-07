# ST\_append

This function appends trajectory points or a sub-trajectory to a trajectory object.

## Syntax

```
trajectory ST_append(trajectory traj, geometry spatial, timestamp[] timespan, text str_attrs_json);
trajectory ST_append(trajectory traj, trajectory tail); 
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The original trajectory object.|
|spatial|The spatial geometry object of the appended trajectory object.|
|timespan|The time array of the appended trajectory object. It can be a timeline.|
|str\_attrs\_json|The attributes of the appended trajectory object. For more information, see [ST\_makeTrajectory](/intl.en-US/Spatio-temporal Database/Trajectory SQL reference/Constructors/ST_makeTrajectory.md).|
|tail|The appended trajectory object.|

## Examples

```
With traj AS (Select ST_makeTrajectory('STPOINT', 'LINESTRING(1 1, 6 6, 9 8)'::geometry, '[2010-01-01 11:30, 2010-01-01 15:00)'::tsrange, '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120, 130, 140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120, 130, 140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120, 130, 140]}, "acceleration": {"type": "string", "length": 20, "nullable" : true,"value": ["120", "130", "140"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 14:30:00 2010", "Fri Jan 01 15:00:00 2010", "Fri Jan 01 15:30:00 2010"]}}, "events": [{"1":"Fri Jan 01 14:30:00 2010"}, {"2":"Fri Jan 01 15:00:00 2010"}, {"3":"Fri Jan 01 15:30:00 2010"}]}') a, ST_makeTrajectory('STPOINT', 'LINESTRING(7 7, 3 4, 1 5)'::geometry, '[2010-01-02 15:30, 2010-01-02 18:00)'::tsrange, '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [121, 131, 141]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [121, 131, 141]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [121, 131, 141]}, "acceleration": {"type": "string", "length": 20, "nullable" : true,"value": ["121", "131", "141"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 02 14:30:00 2010", "Fri Jan 02 15:00:00 2010", "Fri Jan 02 15:30:00 2010"]}}, "events": [{"1":"Fri Jan 02 14:30:00 2010"}, {"2":"Fri Jan 02 15:00:00 2010"}, {"3":"Fri Jan 02 15:30:00 2010"}]}') b)Select ST_Append(a, b) from traj;
                st_append                                                                                                                                                                                                                                                        
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 {"trajectory":{"version":1,"type":"STPOINT","leafcount":6,"start_time":"2010-01-01 11:30:00","end_time":"2010-01-02 18:00:00","spatial":"LINESTRING(1 1,6 6,9 8,7 7,3 4,1 5)","timeline":["2010-01-01 11:30:00","2010-01-01 13:15:00","2010-01-01 15:00:00","2010-01-02 15:30:00","2010-01-02 16:45:00","2010-01-02 18:00:00"],"attributes":{"leafcount":6,"velocity":{"type":"integer","length":2,"nullable":true,"value":[120,130,140,121,131,141]},"accuracy":{"type":"float","length":4,"nullable":false,"value":[120.0,130.0,140.0,121.0,131.0,141.0]},"bearing":{"type":"float","length":8,"nullable":false,"value":[120.0,130.0,140.0,121.0,131.0,141.0]},"acceleration":{"type":"string","length":20,"nullable":true,"value":["120","130","140","121","131","141"]},"active":{"type":"timestamp","length":8,"nullable":false,"value":["2010-01-01 14:30:00","2010-01-01 15:00:00","2010-01-01 15:30:00","2010-01-02 14:30:00","2010-01-02 15:00:00","2010-01-02 15:30:00"]}},"events":[{"1":"2010-01-01 14:30:00"},{"2":"2010-01-01 15:00:00"},{"3":"2010-01-01 15:30:00"},{"1":"2010-01-02 14:30:00"},{"2":"2010-01-02 15:00:00"},{"3":"2010-01-02 15:30:00"}]}}
(1 row)
```

