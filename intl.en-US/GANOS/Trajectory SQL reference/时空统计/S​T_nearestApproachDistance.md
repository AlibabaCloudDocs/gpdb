# S​T\_nearestApproachDistance

This function returns the nearest distance between trajectory objects 1 and 2 within the specified time range.

## Syntax

```
float8 S​T_nearestApproachDistance(trajectory traj, trajectory traj2);
float8 S​T_nearestApproachDistance(trajectory traj, trajectory traj2, tsrange range);
float8 S​T_nearestApproachDistance(trajectory traj, trajectory traj2, timestamp t1, timestamp t2);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj1|Trajectory object 1.|
|traj2|Trajectory object 2.|
|t1|The start time.|
|t2|The end time.|
|range|The time range.|

## Examples

```
Select ST_nearestApproachDistance((Select traj from traj_table where id=1), (Select traj from traj_table where id=2), '2010-1-1 13:00:00', '2010-1-1 14:00:00');
```

