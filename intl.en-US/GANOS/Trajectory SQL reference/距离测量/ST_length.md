# ST\_length

This function returns the total length of a trajectory object, in meters.

## Syntax

```
float8 ST_length(trajectory traj, integer srid default 0);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|
|srid|The spatial reference system identifier \(SRID\) of the trajectory object. Default value: 0 \(4326\).|

## Description

This function computes the spherical length in meters based on the SRID of the trajectory object. You can specify the SRID for a trajectory object by setting the srid parameter. If not specified, the SRID is 4326 by default.

## Examples

```
select st_length(traj) from traj where id = 2;
    st_length     
------------------
 13494.6660605311
(1 row)
```

