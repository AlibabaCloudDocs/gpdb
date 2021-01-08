# ST\_equals

This function returns true if trajectory objects 1 and 2 are spatially equal within the specified time range.

## Syntax

```
boolean ST_equals(trajectory traj1, trajectory traj2, tsrange range);
boolean ST_equals(trajectory traj1, trajectory traj2, timestamp t1, timestamp t2);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|
|t1|The start time.|
|t2|The end time.|
|range|The time range.|

## Examples

```
Select ST_equals((Select traj from traj_table where id=1), (Select traj from traj_table where id=2), '2010-1-1 13:00:00', '2010-1-1 14:00:00');
```

