# ST\_\{T\|2D\|2DT\|3D\|3DT\}Contains

This function is used to check whether the first specified object includes the second specified object on a specified axis.

## Syntax

```
bool ST_TContains(tsrange r, trajectory traj);
bool ST_TContains(trajectory traj, tsrange r);
bool ST_2DContains(geometry geom, trajectory traj);
bool ST_2DContains(trajectory traj, geometry geom);
bool ST_2DContains(geometry geom, trajectory traj, timestamp ts, timestamp te);
bool ST_2DContains(trajectory traj, geometry geom, timestamp ts, timestamp te);
bool ST_{2D|2DT|3D|3DT}Contains(boxndf box, trajectory traj);
bool ST_{2D|2DT|3D|3DT}Contains(boxndf box, trajectory traj, timestamp ts, timestamp te);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|geom|The geometry that you want to compare.|
|traj|The trajectory that you want to compare, or the original trajectory that includes the sub-trajectory that you want to compare.|
|box|The bounding box that you want to compare.|
|r|The time range to query.|
|ts|The beginning of the time range over which you want to extract sub-trajectories. This parameter is optional.|
|te|The end of the time range over which you want to extract sub-trajectories. This parameter is optional.|

## Description

This function allows you to check whether the first specified object includes the second specified object on a specified axis.

-   If you specify a geometry, this function compares the two-dimensional projections of the complete trajectories or the sub-trajectories over a specified time range. Then, this function checks whether the geometry includes or is included in the other specified object.
-   If the first specified object is a bounding box and the second specified object is a trajectory, this function compares the bounding box and the trajectory to check whether the trajectory or its sub-trajectory over a specified time range is included in the bounding box in all dimensions. If the bounding box, trajectory, or sub-trajectory does not have a specified dimension, this function considers this dimension to have any value. In this case, the axis in this dimension automatically meets the specified condition.

**Note:** This function is not supported for geometry types such as POLYHEDRALSURFACE.

## Example

```
WITH traj AS(
    Select ST_makeTrajectory('STPOINT', 'LINESTRING(0 0, 50 50, 100 100)'::geometry,
                             ('[' || ST_PGEpochToTS(0) || ',' || ST_PGEpochToTS(100) || ')')::tsrange,
                             '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120,130,140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120,130,140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120,130,140]}, "acceleration": {"type": "string", "length": 20, "nullable" : true,"value": ["120","130","140"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 11:35:00 2010", "Fri Jan 01 12:35:00 2010", "Fri Jan 01 13:30:00 2010"]}}, "events": [{"2" : "Fri Jan 02 15:00:00 2010"}, {"3" : "Fri Jan 02 15:30:00 2010"}]}') b,
           ST_MakeBox3dt(0,0,0,ST_PgEpochToTS(0), 50,50,50, ST_PgEpochToTS(49)) a
)
SELECT ST_2dContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(49)), ST_3dContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(49)),
       ST_2dtContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(49)), ST_3dtContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(49)),
       ST_2dContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(50)), ST_3dContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(50)),
       ST_2dtContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(50)), ST_3dtContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(50)),
       ST_2dContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(70)), ST_3dContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(70)),
       ST_2dtContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(70)), ST_3dtContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(70)) from traj;
 st_2dcontains | st_3dcontains | st_2dtcontains | st_3dtcontains | st_2dcontains | st_3dcontains | st_2dtcontains | st_3dtcontains | st_2dcontains | st_3dcontains | st_2dtcontains | st_3dtcontains 
---------------+---------------+----------------+----------------+---------------+---------------+----------------+----------------+---------------+---------------+----------------+----------------
 t             | t             | t              | t              | t             | t             | f              | f              | f             | f             | f              | f
```

