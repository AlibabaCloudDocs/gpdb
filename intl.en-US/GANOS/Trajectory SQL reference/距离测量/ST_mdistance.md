# ST\_mdistance

This function returns an array of the Euclidean distance between two trajectory objects at the same time point.

## Syntax

```
float[] ST_mdistance(trajectory traj1, trajectory traj2);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj1|The trajectory object 1.|
|traj2|The trajectory object 2.|

## Description

The distance has not been standardized.

## Examples

```
Select ST_mDistance((Select traj from traj_table where id=1), (Select traj from traj_table where id=2));
```

