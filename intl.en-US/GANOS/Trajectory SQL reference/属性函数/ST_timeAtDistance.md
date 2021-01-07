# ST\_timeAtDistance

This function returns the time point when the movement arrives at a spatial point from the start point for the specified distance.

## Syntax

```
timestamp[] ST_timeAtDistance(trajectory traj, float8 d);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|
|d|The distance of movement.|

This function returns an array of timestamps. Multiple spatial points may be at the same distance from the start point.

## Examples

```
Select ST_timeAtDistance(traj, 100) from traj_table;
```

