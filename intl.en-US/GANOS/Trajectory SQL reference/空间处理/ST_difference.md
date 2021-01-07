# ST\_difference

This function returns a new trajectory object that indicates the difference between a trajectory object within the specified time range and the specified geometry object.

## Syntax

```
trajectory ST_difference(trajectory traj, tsrange range, geometry g);
trajectory ST_difference(trajectory traj, timestamp t1, timestamp t2, geometry g);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|
|t1|The start time.|
|t2|The end time.|
|range|The time range.|
|g|The specified geometry object.|

## Example

```
Select ST_difference(traj, '2010-1-1 13:00:00', '2010-1-1 14:00:00', 'LINESTRING(0 0, 5 5, 9 9)'::geometry) from traj_table;
```

