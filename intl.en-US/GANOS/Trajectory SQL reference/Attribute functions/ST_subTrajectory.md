# ST\_subTrajectory

This function returns the sub-trajectory of a trajectory object within the specified time range.

## Syntax

```
trajectory ST_subTrajectory(trajectory traj, timestamp starttime, timestamp endtime); 
trajectory ST_subTrajectory(trajectory traj, tsrange range);
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
Select ST_subTrajectory(traj, '2010-1-11 02:45:30', '2010-1-11 03:00:00') FROM traj_table;
```

