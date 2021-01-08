# ST\_\{2D\|2DT\|3D\|3DT\}DWithin

This function is used to check whether the distance between two objects on a specified axis is less than or equal to a specified threshold.

## Syntax

```
bool ST_{2D|3D}DWithin(geometry geom, trajectory traj, float8 dist);
bool ST_{2D|3D}DWithin(trajectory traj, geometry geom, float8 dist);
bool ST_{2D|3D}DWithin(geometry geom, trajectory traj, timestamp ts, timestamp te, float8 dist);
bool ST_{2D|3D}DWithin(trajectory traj, geometry geom, timestamp ts, timestamp te, float8 dist);
bool ST_{2D|2DT|3D|3DT}DWithin(boxndf box, trajectory traj, float8 dist);
bool ST_{2D|2DT|3D|3DT}DWithin(trajectory traj, boxndf box, float8 dist);
bool ST_{2D|2DT|3D|3DT}DWithin(boxndf box, trajectory traj, timestamp ts, timestamp te, float8 dist);
bool ST_{2D|2DT|3D|3DT}DWithin(trajectory traj, boxndf box, timestamp ts, timestamp te, float8 dist);
bool ST_{2D|2DT|3D|3DT}DWithin(trajectory traj1, trajectory traj2, float8 dist);
bool ST_{2D|2DT|3D|3DT}DWithin(trajectory traj1, trajectory traj2, timestamp ts, timestamp te, float8 dist);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|geom|The geometry that you want to compare.|
|traj|The trajectory that you want to compare, or the original trajectory that includes the sub-trajectory that you want to compare.|
|traj1|The trajectory that you want to compare, or the original trajectory that includes the sub-trajectory that you want to compare.|
|traj2|The trajectory that you want to compare, or the original trajectory that includes the sub-trajectory that you want to compare.|
|box|The bounding box that you want to compare.|
|ts|The beginning of the time range over which you want to extract sub-trajectories. This parameter is optional.|
|te|The end of the time range over which you want to extract sub-trajectories. This parameter is optional.|
|dist|The threshold for the distance between the two objects.|

## Description

-   For 2D and 3D operations, this function checks whether the distance between the two- or three-dimensional projections of two objects is less than or equal to the specified threshold.
-   For 2DT and 3DT operations, this function checks whether the distance between two objects in a specified dimension at a specified point in time is less than or equal to the specified threshold.

If the function includes the ts and te parameters, it indicates that the trajectory objects \(traj, or traj1 and traj2\) of ST\_ndDWithin are determined as sub-trajectories within the \[ts,te\] time period. Otherwise, it indicates that the trajectory objects \(traj, or traj1 and traj2\) to be determined are complete trajectories.

The `ST_DistanceWithin(..)` function delivers the same effect as the `ST_2DDWithin(...)` function \(if one of the specified objects is a geometry\) and the `ST_3DTDWithin(...)` function \(if both the specified objects are trajectories\).

## Example

```
WITH traj AS(
    Select ST_makeTrajectory('STPOINT', 'LINESTRING(0 0 10, 50 0 10, 100 0 10)'::geometry,
                             ('[' || ST_PGEpochToTS(0) || ',' || ST_PGEpochToTS(100) || ')')::tsrange,
                             '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120,130,140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120,130,140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120,130,140]}, "acceleration": {"type": "string", "length": 20, "nullable" : true,"value": ["120","130","140"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 11:35:00 2010", "Fri Jan 01 12:35:00 2010", "Fri Jan 01 13:30:00 2010"]}}, "events": [{"2" : "Fri Jan 02 15:00:00 2010"}, {"3" : "Fri Jan 02 15:30:00 2010"}]}') a,
           ST_makeTrajectory('STPOINT', 'LINESTRING(0 20, 50 20, 100 20)'::geometry,
                             ('[' || ST_PGEpochToTS(0) || ',' || ST_PGEpochToTS(100) ||')')::tsrange,
                             '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120,130,140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120,130,140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120,130,140]}, "acceleration": {"type": "string", "length": 20, "nullable" : true,"value": ["120","130","140"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 11:35:00 2010", "Fri Jan 01 12:35:00 2010", "Fri Jan 01 13:30:00 2010"]}}, "events": [{"2" : "Fri Jan 02 15:00:00 2010"}, {"3" : "Fri Jan 02 15:30:00 2010"}]}') b
)
SELECT ST_2dDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(49), 19), ST_3dDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(49), 19),
       ST_2dDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(50), 20), ST_3dDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(50), 20),
       ST_2dDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(70), 21), ST_3dDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(70), 21),
       ST_2dtDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(49), 19), ST_3dtDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(49), 19),
       ST_2dtDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(50), 20), ST_3dtDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(50), 20),
       ST_2dtDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(70), 21), ST_3dtDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(70), 21) from traj;
NOTICE:  One or both of the geometries is missing z-value. The unknown z-value will be regarded as "any value"
NOTICE:  One or both of the geometries is missing z-value. The unknown z-value will be regarded as "any value"
 st_2ddwithin | st_3ddwithin | st_2ddwithin | st_3ddwithin | st_2ddwithin | st_3ddwithin | st_2dtdwithin | st_3dtdwithin | st_2dtdwithin | st_3dtdwithin | st_2dtdwithin | st_3dtdwithin 
--------------+--------------+--------------+--------------+--------------+--------------+---------------+---------------+---------------+---------------+---------------+---------------
 f            | f            | t            | t            | t            | t            | f             | f             | t             | t             | t             | t
```

