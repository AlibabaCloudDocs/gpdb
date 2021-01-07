# ST\_cumulativeDistanceAtTime

This function returns the cumulative distance of movement from the start point to the spatial point arrived at the specified time point.

## Syntax

```
float8 ST_cumulativeDistanceAtTime(trajectory traj, timestamp t) ;
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|
|t|The time point.|

## Examples

```
Select ST_cumulativeDistanceAtTime(traj, '2011-11-1 04:30:00') from traj_table;
```

