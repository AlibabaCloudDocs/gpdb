# ST\_attrTimestampFilter

This function filters all trajectory points to obtain the trajectory points whose values for the specified attribute field meet a filter condition based on a fixed value or a value range and returns an array of the attribute field values of these points.

## Syntax

```
timestamp[] ST_attrTimestampFilter(trajectory traj, cstring attr_field_name,cstring operator, timestamp value);
timestamp[] ST_attrTimestampFilter(trajectory traj, cstring attr_field_name,cstring operator, timestamp value1, timestamp value2);
```

## Parameter

|Option|Description|
|------|-----------|
|traj|The trajectory object.|
|attr\_field\_name|The name of the specified attribute field.|
|operator|Filter, `'='`， `'! ='`， `'>'`， `'<'`， `'>='`， `'<='`， `'[]'`， `'(]'`， `'[)'`， `'()'`.|
|value and value1|The fixed value of the attribute field and the minimum fixed value of the attribute field.|
|value2|The maximum fixed value of the attribute field.|

## Examples

```
insert into traj values(3,ST_makeTrajectory('STPOINT'::leaftype, st_geomfromtext('LINESTRING (114 35, 115 36, 116 37)', 4326), ARRAY['2010-01-01 14:30'::timestamp, '2010-01-01 15:00', '2010-01-01 15:30'], '{"leafcount": 3, "attributes": {"heading": {"type": "timestamp", "nullable": false,"value":["Fri Jan 01 14:30:00 2010", "Fri Jan 01 15:00:00 2010", "Fri Jan 01 15:30:00 2010"]}}}'));
select st_attrTimestampFilter(traj, 'heading', '>', 'Fri Jan 01 15:00:00 2010'::timestamp) from traj where id = 3;
 st_attrtimestampfilter  
------------------------- 
{"2010-01-01 15:30:00"}
(1 row)
```

