# ST\_leafCount

This function returns the number of leaves \(trajectory points\) in a trajectory object.

## Syntax

```
integer ST_leafCount(trajectory traj);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|

## Examples

```
select st_leafCount(traj) from traj where id = 2;
 st_leafcount 
--------------           
16
```

