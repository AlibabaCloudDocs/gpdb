# ST\_samplingInterval

This function returns the sampling interval.

## Syntax

```
interval ST_samplingInterval(trajectory traj);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|

## Examples

```
select ST_samplingInterval(ST_MakeTrajectory('STPOINT'::leaftype, st_geomfromtext('LINESTRING (114 35, 115 36, 116 37)', 4326), '[2010-01-01 14:30, 2010-01-01 15:30)'::tsrange, '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable": true,"value": [120, 130, 140]}, "accuracy": {"type": "float", "length": 4, "nullable": false,"value": [120, 130, 140]}, "bearing": {"type": "float", "length": 8, "nullable": false,"value": [120, 130, 140]}, "acceleration": {"type": "string", "length": 20, "nullable": true,"value": ["120", "130", "140"]}, "active": {"type": "timestamp", "nullable": false,"value": ["Fri Jan 01 14:30:00 2010", "Fri Jan 01 15:00:00 2010", "Fri Jan 01 15:30:00 2010"]}}, "events": [{"1":"Fri Jan 01 14:30:00 2010"}, {"2":"Fri Jan 01 15:00:00 2010"}, {"3":"Fri Jan 01 15:30:00 2010"}]}'));
 st_samplinginterval 
---------------------
 @ 20 mins
(1 row)
```

