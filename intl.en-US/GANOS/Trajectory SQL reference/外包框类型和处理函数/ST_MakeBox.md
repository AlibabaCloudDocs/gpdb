# ST\_MakeBox

This function is used to query the bounding box of an object over a specified time range.

## Syntax

```
boxndf ST_MakeBox(geometry geom);
boxndf ST_MakeBox(trajectory traj);
boxndf ST_MakeBox(geometry geom, timestamp ts, timestamp te);
boxndf ST_MakeBox(trajectory traj, timestamp ts, timestamp te);
boxndf ST_MakeBox(timestamp ts, timestamp te);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|geom|The geometry whose bounding box you want to query.|
|traj|The trajectory whose bounding box you want to query.|
|ts|The beginning of the time range to query.|
|te|The end of the time range to query.|

## Description

A bounding box is the minimal multi-dimensional rectangle that contains a spatio-temporal object.

-   If you specify a two- or three-dimensional spatial object, this function returns the bounding box of the object based on the dimensions of the object.
-   If you specify a spatial object and a time range by using the `ST_MakeBox(geometry geom, timestamp ts, timestamp te)` syntax, this function returns the bounding box of the object over the time range.
-   If you specify a trajectory but do not specify a time range, this function returns the bounding box of the complete trajectory. If you specify a trajectory and a time range, this function returns the bounding box of the sub-trajectory over the time range.
-   Bounding boxes use the FLOAT data type. Therefore, this function may return a bounding box that is slightly larger than specified by the parameter settings. For example, the returned lower bound is slightly smaller than the actual lower bound, or the returned upper bound is slightly larger than the actual upper bound.

## Example

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

