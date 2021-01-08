# ST\_lcsSubDistance

This function computes the distance between a longest common sub-sequence \(LCSS\) sub-trajectory and a trajectory object.

## Syntax

```
float8 ST_lcsSubDistance(trajectory traj1, trajectory traj2, float8 dist, distanceUnit unit default 'M');
float8 ST_lcsSubDistance(trajectory traj1, trajectory traj2, float8 dist, interval lag, distanceUnit unit default 'M');
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj1|The trajectory object 1.|
|traj2|The trajectory object 2.|
|dist|The distance tolerance between two trajectory points. Unit: meters.|
|lag|The time tolerance between two trajectory points.|
|unit|The unit of the distance. Valid values: -   'M': meters.
-   'KM': kilometers.
-   'D': degrees. This value is valid only when the spatial reference system identifier \(SRID\) of a trajectory object is WGS84 \(4326 by default\). |

## Description

This function computes the sub-trajectory of trajectory object 1 in which trajectory points are consistent with those in the LCSS sub-trajectory, and then returns the ratio between the total number of trajectory points in the sub-trajectory of trajectory object 1 and the number of trajectory points in the LCSS sub-trajectory.

![Ratio calculation algorithm](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7429209951/p50874.png)

As shown in the preceding figure, trajectory points 2, 3, and 5 are in the LCSS sub-trajectory. Trajectory points 2, 3, 4, and 5 are in the sub-trajectory of trajectory object 1. In this case, 1 - 3/4 = 0.25. This function returns 0.25.

A smaller value indicates a higher similarity between the LCSS sub-trajectory and trajectory object 1.

Generally, a trajectory object has a valid SRID. If not specified, the SRID is 4326 by default.

## Examples

```
With traj AS (
    Select ST_makeTrajectory('STPOINT', 'LINESTRINGZ(114.000528 33.588163 54.87, 114.000535 33.588235 54.85, 114.000447 33.588272 54.69, 114.000348 33.588287 54.73, 114.000245 33.588305 55.26, 114.000153 33.588305 55.3)'::geometry,
                             ARRAY['2010-01-01 11:30'::timestamp, '2010-01-01 11:31', '2010-01-01 11:32', '2010-01-01 11:33','2010-01-01 11:34','2010-01-01 11:35'], NULL) a,
           ST_makeTrajectory('STPOINT', 'LINESTRINGZ(114.000529 33.588163 54.87, 114.000535 33.578235 54.85, 114.000447 33.578272 54.69, 114.000348 33.578287 54.73, 114.000245 33.578305 55.26, 114.000163 33.588305 55.3)'::geometry,
                             ARRAY['2010-01-01 11:29:58'::timestamp, '2010-01-01 11:31:02', '2010-01-01 11:33', '2010-01-01 11:33:09','2010-01-01 11:34','2010-01-01 11:34:30'], NULL) b)
Select st_LCSSubDistance(a, b, 100) from traj;

st_lcssubdistance 
------------------------
 0.66666666666666666667 
 (1 row)

 With traj AS (
    Select ST_makeTrajectory('STPOINT', 'LINESTRINGZ(114.000528 33.588163 54.87, 114.000535 33.588235 54.85, 114.000447 33.588272 54.69, 114.000348 33.588287 54.73, 114.000245 33.588305 55.26, 114.000153 33.588305 55.3)'::geometry,
                             ARRAY['2010-01-01 11:30'::timestamp, '2010-01-01 11:31', '2010-01-01 11:32', '2010-01-01 11:33','2010-01-01 11:34','2010-01-01 11:35'], NULL) a,
           ST_makeTrajectory('STPOINT', 'LINESTRINGZ(114.000529 33.588163 54.87, 114.000535 33.578235 54.85, 114.000447 33.578272 54.69, 114.000348 33.578287 54.73, 114.000245 33.578305 55.26, 114.000163 33.588305 55.3)'::geometry,
                             ARRAY['2010-01-01 11:29:58'::timestamp, '2010-01-01 11:31:02', '2010-01-01 11:33', '2010-01-01 11:33:09','2010-01-01 11:34','2010-01-01 11:34:30'], NULL) b)
Select st_LCSSubDistance(a, b, 100, interval '30 seconds') from traj;
st_lcssubdistance 
-------------------
0.666666666666667
(1 row)
```

