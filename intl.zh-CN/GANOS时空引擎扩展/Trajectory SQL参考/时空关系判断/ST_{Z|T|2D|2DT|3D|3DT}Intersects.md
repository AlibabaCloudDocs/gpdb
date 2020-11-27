# ST\_\{Z\|T\|2D\|2DT\|3D\|3DT\}Intersects

ST\_ndIntersects函数，判断指定的两个对象在指定的坐标轴上是否相交。

## 语法

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

## 参数

|参数名称|描述|
|----|--|
|geom|需要判断的几何对象。|
|traj|需要判断的轨迹对象，或者产生子轨迹的原始轨迹。|
|traj1|需要判断的轨迹对象，或者产生子轨迹的原始轨迹。|
|traj2|需要判断的轨迹对象，或者产生子轨迹的原始轨迹。|
|box|需要判断的外包框对象。|
|ts|如存在，表示按此时间作为开始时间截取子轨迹。|
|te|如存在，表示按此时间作为结束时间截取子轨迹。|

## 描述

在指定维度上，返回需要判断的两个对象（即第一个和第二个参数）的相交结果。对于没有值的维度，将其视为任意值进行判断。

如果函数包含ts和te参数，表示需要判断的对象（traj，或traj1和traj2）是在\[ts,te\]时间段内的子轨迹；如果函数不包含ts和te参数，表示需要判断的对象（traj，或traj1和traj2）是完整的轨迹。

直接调用`ST_Intersects(...)`函数相当于调用`ST_2DIntersects(...)`（若前两个参数其中一个为几何对象）或`ST_3DTIntersects(...)`（若前两个参数均为轨迹对象）。

## 示例

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

