# ST\_timeToDistance

This function returns a line chart in which the time is the x-axis and the Euclidean distance is the y-axis.

## Syntax

```
geometry ST_timeToDistance(trajectory traj);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|

## Examples

```
Select ST_timeToDistance(traj) from traj_table;
```

