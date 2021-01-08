# ST\_trajAttrsMeanMax

This function returns the maximum average value of an attribute field by using the MEAN-MAX algorithm.

## Syntax

```
SETOF record ST_trajAttrsMeanMax(trajectory traj, cstring attr_field_name, out interval duration, out float8 max);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|
|attr\_field\_name|The name of the specified attribute field.|

## Description

The MEAN-MAX algorithm uses a sliding window to compute the average value of the specified attribute field in each time range specified by the window, and then returns the maximum value of all the average values.

![MEAN-MAX algorithm](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/0429209951/p50858.png)

This function supports only attribute fields of the integer and float types. The values of the specified attribute field cannot be null.

## Examples

```
With traj AS (
    Select ST_makeTrajectory('STPOINT', 'LINESTRING(1 1, 6 6, 9 8, 10 12)'::geometry,
    ARRAY['2010-01-01 11:30'::timestamp, '2010-01-01 12:30', '2010-01-01 13:30', '2010-01-01 14:30'],
    '{"leafcount":4, "attributes":{"velocity": {"type": "float", "length": 8,"nullable": true,"value": [120.0, 130.0, 140.0, 120.0]}, "power": {"type": "float", "length": 4,"nullable": true,"value": [120.0, 130.0, 140.0, 120.0]}}}') a)
Select st_trajAttrsMeanMax(a, 'velocity') from traj;
 st_trajattrsmeanmax 
---------------------
 ("@ 1 hour",135)
 ("@ 2 hours",130)
 ("@ 3 hours",127.5)
(3 rows)

 With traj AS (
    Select ST_makeTrajectory('STPOINT', 'LINESTRING(1 1, 6 6, 9 8, 10 12)'::geometry,
    ARRAY['2010-01-01 11:30'::timestamp, '2010-01-01 12:30', '2010-01-01 13:30', '2010-01-01 14:30'],
    '{"leafcount":4, "attributes":{"velocity": {"type": "float", "length": 8,"nullable": true,"value": [120.0, 130.0, 140.0, 120.0]}, "power": {"type": "float", "length": 4,"nullable": true,"value": [120.0, 130.0, 140.0, 120.0]}}}') a)
Select (st_trajAttrsMeanMax(a, 'velocity')).* from traj;
 duration |  max  
----------+-------
 01:00:00 |   135
 02:00:00 |   130
 03:00:00 | 127.5
(3 rows)
```

