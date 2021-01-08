# ST\_lcsSimilarity

This function computes the similarity between two trajectory objects based on the longest common sub-sequence \(LCSS\) algorithm.

## Syntax

```
integer ST_lcsSimilarity(trajectory traj1, trajectory traj2, float8 dist, distanceUnit unit default 'M');
integer ST_lcsSimilarity(trajectory traj1, trajectory traj2, float8 dist, interval lag, distanceUnit unit default 'M');
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

The LCSS algorithm is used to compute the maximum similarity between two trajectory objects. The consistency between two trajectory points is determined based on their distance in space and time. This function computes the similarity between two trajectory objects and returns the number of consistent trajectory points.

![Similarity calculation algorithm](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/6429209951/p50869.png)

As shown in the preceding figure, trajectory points 1, 3, and 6 meet the conditions of consistency. In this case, this function returns 3.

Generally, a trajectory object has a valid SRID. If not specified, the SRID is 4326 by default.

## Examples

```
With traj AS (
    Select ST_makeTrajectory('STPOINT', 'LINESTRINGZ(114.000528 33.588163 54.87, 114.000535 33.588235 54.85, 114.000447 33.588272 54.69, 114.000348 33.588287 54.73, 114.000245 33.588305 55.26, 114.000153 33.588305 55.3)'::geometry,
                             ARRAY['2010-01-01 11:30'::timestamp, '2010-01-01 11:31', '2010-01-01 11:32', '2010-01-01 11:33','2010-01-01 11:34','2010-01-01 11:35'], NULL) a,
           ST_makeTrajectory('STPOINT', 'LINESTRINGZ(114.000529 33.588163 54.87, 114.000535 33.578235 54.85, 114.000447 33.578272 54.69, 114.000348 33.578287 54.73, 114.000245 33.578305 55.26, 114.000163 33.588305 55.3)'::geometry,
                             ARRAY['2010-01-01 11:29:58'::timestamp, '2010-01-01 11:31:02', '2010-01-01 11:33', '2010-01-01 11:33:09','2010-01-01 11:34','2010-01-01 11:34:30'], NULL) b)
Select st_LCSSimilarity(a, b, 100) from traj;
  st_lcssimilarity    
-------------------
 2
 (1 row)

 With traj AS (
    Select ST_makeTrajectory('STPOINT', 'LINESTRINGZ(114.000528 33.588163 54.87, 114.000535 33.588235 54.85, 114.000447 33.588272 54.69, 114.000348 33.588287 54.73, 114.000245 33.588305 55.26, 114.000153 33.588305 55.3)'::geometry,
                             ARRAY['2010-01-01 11:30'::timestamp, '2010-01-01 11:31', '2010-01-01 11:32', '2010-01-01 11:33','2010-01-01 11:34','2010-01-01 11:35'], NULL) a,
           ST_makeTrajectory('STPOINT', 'LINESTRINGZ(114.000529 33.588163 54.87, 114.000535 33.578235 54.85, 114.000447 33.578272 54.69, 114.000348 33.578287 54.73, 114.000245 33.578305 55.26, 114.000163 33.588305 55.3)'::geometry,
                             ARRAY['2010-01-01 11:29:58'::timestamp, '2010-01-01 11:31:02', '2010-01-01 11:33', '2010-01-01 11:34:15','2010-01-01 11:34:50','2010-01-01 11:34:30'], NULL) b)
Select st_LCSSimilarity(a, b, 100, interval '30 seconds') from traj;
 st_lcssimilarity   
-------------------
2 
(1 row)
```

