# ST\_nearestApproachPoint

This function returns the spatial point in trajectory object 1 that is nearest to trajectory object 2 within the specified time range.

## Syntax

```
geometry ST_nearestApproachPoint(trajectory traj1, trajectory traj2);
geometry ST_nearestApproachPoint(trajectory traj1, trajectory traj2,tsrange range);
geometry ST_nearestApproachPoint(trajectory traj1, trajectory traj2, timestamp t1, timestamp t2);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj1|The trajectory object 1.|
|traj2|The trajectory object 2.|
|t1|The start time.|
|t2|The end time.|
|range|The time range.|

## Example

```
Select ST_nearestApproachPoint((Select traj from traj_table where id=1), (Select traj from traj_table where id=2), '2010-1-1 13:00:00', '2010-1-1 14:00:00');
```

