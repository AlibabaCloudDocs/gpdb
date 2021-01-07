# ST\_subTrajectorySpatial

This function returns the spatial geometry object of a trajectory object within the specified time range.

## Syntax

```
geometry ST_subTrajectorySpatial(trajectory traj, timestamp starttime, timestamp endtime);
geometry ST_subTrajectorySpatial(trajectory traj, tsrange range);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|
|starttime|The start time.|
|endtime|The end time.|
|range|The time range.|

## Examples

```
Select ST_subTrajectorySpatial(traj, '2010-1-11 02:45:30', '2010-1-11 03:00:00') FROM traj_table;
```

