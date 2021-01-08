# ST\_intersection

This function returns a new trajectory object that indicates the intersection of a trajectory object within the specified time range and the specified geometry object.

## Syntax

```
trajectory[] ST_intersection(trajectory traj, tsrange range, geometry g);
trajectory[] ST_intersection(trajectory traj, timestamp t1, timestamp t2, geometry g);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|
|t1|The start time.|
|t2|The end time.|
|range|The time range.|
|g|The specified geometry object.|

## Description

If the trajectory object and the geometry object intersect at multiple spatial points, this function returns multiple sub-trajectories.

## Example

```
Select ST_intersection(traj, '2010-1-1 13:00:00', '2010-1-1 14:00:00', 'LINESTRING(0 0, 5 5, 9 9)'::geometry) from traj_table;
```

