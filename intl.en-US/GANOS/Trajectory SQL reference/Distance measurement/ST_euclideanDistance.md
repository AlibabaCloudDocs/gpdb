# ST\_euclideanDistance

This function returns the nearest Euclidean distance between two trajectory objects at the same time point.

## Syntax

```
float ST_euclideanDistance(trajectory traj1, trajectory traj2);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj1|The trajectory object 1.|
|traj2|The trajectory object 2.|

## Description

The distance has been standardized.

## Examples

```
Select ST_euclideanDistance((Select traj from traj_table where id=1), (Select traj from traj_table where id=2));
```

