# ST\_\{Z\|T\|2D\|2DT\|3D\|3DT\}Intersects

This function is used to check whether two objects intersect on a specified axis.

## Syntax

```
bool ST_{2D|3D}Intersects(geometry geom, trajectory traj);
bool ST_{2D|3D}Intersects(trajectory traj, geometry geom);
bool ST_{2D|3D}Intersects(geometry geom, trajectory traj, timestamp ts, timestamp te);
bool ST_{2D|3D}Intersects(trajectory traj, geometry geom, timestamp ts, timestamp te);
bool ST_{Z|T|2D|2DT|3D|3DT}Intersects(boxndf box, trajectory traj);
bool ST_{Z|T|2D|2DT|3D|3DT}Intersects(trajectory traj, boxndf box);
bool ST_{Z|T|2D|2DT|3D|3DT}Intersects(boxndf box, trajectory traj, timestamp ts, timestamp te);
bool ST_{Z|T|2D|2DT|3D|3DT}Intersects(trajectory traj, boxndf box, timestamp ts, timestamp te);
bool ST_{2D|2DT|3D|3DT}Intersects(trajectory traj1, trajectory traj2);
bool ST_{2D|2DT|3D|3DT}Intersects(trajectory traj1, trajectory traj2, timestamp ts, timestamp te);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|geom|The geometry that you want to compare.|
|traj|The trajectory that you want to compare, or the original trajectory that includes the sub-trajectory that you want to compare.|
|traj1|The trajectory that you want to compare, or the original trajectory that includes the sub-trajectory that you want to compare.|
|traj2|The trajectory that you want to compare, or the original trajectory that includes the sub-trajectory that you want to compare.|
|box|The bounding box that you want to compare.|
|ts|The beginning of the time range over which you want to extract sub-trajectories. This parameter is optional.|
|te|The end of the time range over which you want to extract sub-trajectories. This parameter is optional.|

## Description

This function allows you to check whether two objects intersect in a specified dimension. If a dimension does not have a specified value, this function considers this dimension to have any value.

If this function contains the ts and te parameters, it compares two objects to check whether the sub-trajectories of the two objects over the specified time range intersect. If this function does not contain the ts or te parameter, it compares two objects to check whether the complete trajectories of the two objects intersect. The two objects are specified by the traj or traj1 parameter and the traj2 parameter.

The `ST_Intersects(...)` function delivers the same effect as the `ST_2DIntersects(...)` function \(if one of the specified objects is a geometry\) or the `ST_3DTIntersects(...)` function \(if both the specified objects are trajectories\).

## Example

```
WITH traj AS(
    SELECT (' {"trajectory":{"version":1,"type":"STPOINT","leafcount":6,"start_time":"2000-01-01 03:15:42","end_time":"2000-01-01 05:16:43",' ||
            '"spatial":"LINESTRING(2 2 0,33.042158099636 36.832684322819 0,47.244002354518 47.230026333034 0,64.978971942887 60.618813472986 0,77.621717839502 78.012496630661 0,80 78 0)",' ||
            '"timeline":["2000-01-01 03:15:42","2000-01-01 03:39:54","2000-01-01 04:04:06","2000-01-01 04:28:18","2000-01-01 04:52:31","2000-01-01 05:16:43"]}}')::trajectory a,
           ('{"trajectory":{"version":1,"type":"STPOINT","leafcount":4,"start_time":"2000-01-01 02:17:58.656079","end_time":"2000-01-01 03:43:59.620923",' ||
            '"spatial":"LINESTRING(40 2 0,15.17549143297 51.766017656152 0,1.444002354518 69.630026333034 0,3 70 0)","timeline":["2000-01-01 02:17:58.656079",' ||
            '"2000-01-01 02:46:38.977693","2000-01-01 03:15:19.299307","2000-01-01 03:43:59.620923"]}}')::trajectory b
)
SELECT ST_2dIntersects(a,b), ST_2dtIntersects(a,b), ST_3dIntersects(a,b), ST_3dtIntersects(a,b) from traj;
 st_2dintersects | st_2dtintersects | st_3dintersects | st_3dtintersects 
-----------------+------------------+-----------------+------------------
 t               | f                | t               | f
```

