# ST\_duration

This function returns the duration of a trajectory object.

## Syntax

```
interval ST_duration(trajectory traj);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|

## Examples

```
select st_duration(traj) from traj where id = 2;
 st_duration 
-------------
 00:42:10
(1 row)
```

