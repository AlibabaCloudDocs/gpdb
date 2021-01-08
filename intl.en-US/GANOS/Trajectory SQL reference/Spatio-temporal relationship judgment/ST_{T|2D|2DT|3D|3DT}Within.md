# ST\_\{T\|2D\|2DT\|3D\|3DT\}Within

This topic describes the ST\_\{T\|2D\|2DT\|3D\|3DT\}Within function, which is used to determine whether the first specified object is included in the second specified object on a specified axis.

## Syntax

```
bool ST_TWithin(tsrange r, trajectory traj);
bool ST_TWithin(trajectory traj, tsrange r);
bool ST_2DWithin(geometry geom, trajectory traj);
bool ST_2DWithin(trajectory traj, geometry geom);
bool ST_2DWithin(geometry geom, trajectory traj, timestamp ts, timestamp te);
bool ST_2DWithin(trajectory traj, geometry geom, timestamp ts, timestamp te);
bool ST_{2D|2DT|3D|3DT}Within(trajectory traj, boxndf box);
bool ST_{2D|2DT|3D|3DT}Within(trajectory traj, boxndf box, timestamp ts, timestamp te);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|geom|The geometry that you want to compare.|
|traj|The trajectory that you want to compare, or the original trajectory that includes the sub-trajectory that you want to compare.|
|box|The bounding box that you want to compare.|
|r|The time range that you want to compare.|
|ts|The beginning of the time range over which you want to extract sub-trajectories. This parameter is optional.|
|te|The end of the time range over which you want to extract sub-trajectories. This parameter is optional.|

## Description

This function allows you to determine whether the first specified object is included in the second specified object. This function works in the same way as the ST\_ndContains function.

**Note:** This function is not supported for geometry types such as POLYHEDRALSURFACE.

## Example

```
WITH traj AS(
    SELECT (' {"trajectory":{"version":1,"type":"STPOINT","leafcount":6,"start_time":"2000-01-01 03:15:42","end_time":"2000-01-01 05:16:43",' ||
            '"spatial":"LINESTRING(2 2 0,33.042158099636 36.832684322819 0,47.244002354518 47.230026333034 0,64.978971942887 60.618813472986 0,77.621717839502 78.012496630661 0,80 78 0)",' ||
            '"timeline":["2000-01-01 03:15:42","2000-01-01 03:39:54","2000-01-01 04:04:06","2000-01-01 04:28:18","2000-01-01 04:52:31","2000-01-01 05:16:43"]}}')::trajectory a,
           'LINESTRING(2 2 0,33.042158099636 36.832684322819 0,47.244002354518 47.230026333034 0,64.978971942887 60.618813472986 0,77.621717839502 78.012496630661 0,80 78 0)'::geometry b
)
SELECT ST_2dWithin(b,a) from traj;
 st_2dwithin 
-------------
 t
```

