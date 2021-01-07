# ST\_attrDefinition

This function returns the definitions of attributes in a trajectory object.

## Syntax

```
t​ext ST_attrDefinition(trajectory traj);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|

## Examples

```
With traj AS (
  select '{"trajectory":{"version":1,"type":"STPOINT","leafcount":2,"start_time":"2010-01-01 11:30:00","end_time":"2010-01-01 12:30:00","spatial":"SRID=4326;LINESTRING(1 1,3 5)","timeline":["2010-01-01 11:30:00","2010-01-01 12:30:00"],"attributes":{"leafcount":2,"velocity":{"type":"integer","length":4,"nullable":true,"value":[1,null]},"speed":{"type":"float","length":8,"nullable":true,"value":[null,1.0]},"angle":{"type":"string","length":64,"nullable":true,"value":["test",null]}, "tngel2":{"type":"timestamp","length":8,"nullable":true,"value":["2010-01-01 12:30:00",null]},"bearing":{"type":"bool","length":1,"nullable":true,"value":[null,true]}}}}'::trajectory a)
Select st_attrDefinition(a) from traj;
                                                                                                                                      st_attrdefinition                                                                                                                                      
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 {"size":5,"velocity":{"type":"integer","length":4,"nullable":true},"speed":{"type":"float","length":8,"nullable":true},"angle":{"type":"string","length":64,"nullable":true},"tngel2":{"type":"timestamp","length":8,"nullable":true},"bearing":{"type":"bool","length":1,"nullable":true}}
(1 row)
```

