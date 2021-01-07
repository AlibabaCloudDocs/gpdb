# ST\_trajectoryTemporal

This function returns the timeline of a trajectory object.

## Syntax

```
text ST_trajectoryTemporal(trajectory traj);
text ST_trajTemporal(trajectory traj);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|

## Examples

```
select ST_trajectoryTemporal(ST_MakeTrajectory('STPOINT'::leaftype, st_geomfromtext('LINESTRING (114 35, 115 36, 116 37)', 4326), '[2010-01-01 14:30, 2010-01-01 15:30)'::tsrange, null));
                              st_trajectorytemporal                             
--------------------------------------------------------------------------------
 {"timeline":["2010-01-01 14:30:00","2010-01-01 15:00:00","2010-01-01 15:30:00"]}
(1 row)
```

