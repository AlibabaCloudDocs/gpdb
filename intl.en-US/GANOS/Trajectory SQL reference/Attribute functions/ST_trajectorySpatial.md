# ST\_trajectorySpatial

This function returns the spatial geometry object of a trajectory object.

## Syntax

```
geometry ST_trajectorySpatial(trajectory traj);
geometry ST_trajSpatial(trajectory traj);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|

## Example

```
Select ST_AsText(ST_trajectorySpatial(traj)) FROM traj_table;
            st_astext
----------------------------------
 LINESTRING(114 35,115 36,116 37)
```

