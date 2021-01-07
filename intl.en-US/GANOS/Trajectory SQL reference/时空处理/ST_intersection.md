# ST\_intersection

This function returns a new trajectory object that indicates the intersection of trajectory objects 1 and 2 within the specified time range.

## Syntax

```
geometry ST_intersection(trajectory traj1, trajectory traj2, tsrange range);
geometry ST_intersection(trajectory traj1, trajectory traj2, timestamp t1, timestamp t2);
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
Select ST_intersection((Select traj from traj_table where id=1), (Select traj from traj_table where id=2), '2010-1-1 13:00:00', '2010-1-1 14:00:00');
```

