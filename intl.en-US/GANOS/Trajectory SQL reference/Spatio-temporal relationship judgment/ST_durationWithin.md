# ST\_durationWithin

This function returns true if the difference between the time when trajectory objects 1 and 2 intersect at the same spatial point within the specified time range is within the reference interval.

## Syntax

```
boolean ST_durationWithin(trajectory traj1, trajectory traj2, tsrange range, interval i);
boolean ST_durationWithin(trajectory traj1, trajectory traj2, timestamp t1, timestamp t2, interval i);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|
|t1|The start time.|
|t2|The end time.|
|range|The time range.|
|i|The reference interval.|

## Description

If two trajectory objects intersect at the same spatial point multiple times, this function returns true so long as any time difference is within the reference interval.

## Examples

```
Select ST_durationWithin((Select traj from traj_table where id=1), (Select traj from traj_table where id=2), '2010-1-1 13:00:00', '2010-1-1 14:00:00', INTERVAL '30s');
```

