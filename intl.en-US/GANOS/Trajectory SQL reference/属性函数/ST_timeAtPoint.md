# ST\_timeAtPoint

This function returns a set of time points when a trajectory object passes through the specified spatial point.

## Syntax

```
timestamp[] ST_timeAtPoint(trajectory traj, geometry g);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|
|g|The spatial point.|

## Example

```
Select ST_timeAtPoint(traj, st_geomfromtext('POINT(115 36)')) from traj_table;
     st_timeatpoint      
-------------------------
 {"2010-01-01 15:00:00"}
```

