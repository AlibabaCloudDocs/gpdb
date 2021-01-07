# ST\_timeAtCumulativeDistance

This function returns the time point when the movement arrives at a spatial point from the start point for the specified cumulative distance.

## Syntax

```
timestamp ST_timeAtCumulativeDistance(trajectory traj, float d) ;
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|
|d|The cumulative distance of movement.|

## Examples

```
Select ST_timeAtCumulativeDistance(traj, 100.0) from traj_table;
```

