# ST\_deviation

This function returns the deviation of the processed trajectory object from the original trajectory object.

## Syntax

```
float8 ST_deviation(trajectory traj, trajectory after_oper_traj);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The original trajectory object.|
|after\_oper\_traj|The processed trajectory object, such as the compressed trajectory object.|

## Examples

```
select st_deviation(traj, st_compress(traj,0.001)) from traj_test;
    st_deviation     
--------------------- 
0.00919177345596219
(1 row)
```

