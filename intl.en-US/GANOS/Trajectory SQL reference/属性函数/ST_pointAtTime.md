# ST\_pointAtTime

This function returns the spatial geometry object of a trajectory object at the specified time point.

## Syntax

```
geometry ST_pointAtTime(trajectory traj, timestamp t);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|
|t|The time point.|

This function returns a spatial point.

## Examples

```
Select ST_pointAtTime(traj, '2010-1-11 23:40:00') from traj_table;
```

