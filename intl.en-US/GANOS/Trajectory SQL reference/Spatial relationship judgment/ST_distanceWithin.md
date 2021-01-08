# ST\_distanceWithin

This function returns true if the distance between a trajectory object within the specified time range and the specified geometry object is within the reference distance.

## Syntax

```
boolean ST_distanceWithin(trajectory traj, tsrange range, geometry g, float8 d);
boolean ST_distanceWithin(trajectory traj, timestamp t1, timestamp t2, geometry g, float8 d);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|
|t1|The start time.|
|t2|The end time.|
|range|The time range.|
|g|The specified geometry object.|
|d|The reference distance.|

## Example

```
Select ST_distanceWithin(traj, '2010-1-1 13:00:00', '2010-1-1 14:00:00', 'LINESTRING(0 0, 5 5, 9 9)'::geometry, 10) from traj_table;
```

