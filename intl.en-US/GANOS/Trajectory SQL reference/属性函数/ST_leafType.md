# ST\_leafType

This function returns the leaf type of a trajectory object.

## Syntax

```
text ST_leafType(trajectory traj);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|

Only the ST\_POINT type is supported.

## Examples

```
select st_leafType(traj) from traj where id = 3;
 st_leaftype
 ------------- 
STPOINT
(1 row)
```

