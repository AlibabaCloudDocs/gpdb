# ST\_\{T\|2D\|2DT\|3D\|3DT\}Within

ST\_ndWithin函数，判断第一个参数所对应的对象在指定的坐标轴上是否被包含于第二个参数所对应的对象。

## 语法

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

## 参数

|参数名称|描述|
|----|--|
|geom|需要判断的几何对象。|
|traj|需要判断的轨迹对象，或者产生子轨迹的原始轨迹。|
|box|需要判断的外包框对象。|
|r|需要判断的时间范围。|
|ts|如存在，表示按此时间作为开始时间截取子轨迹。|
|te|如存在，表示按此时间作为结束时间截取子轨迹。|

## 描述

判断第一个参数是否被包含于第二个参数，等价于将第一个参数与第二个参数交换的ST\_ndContains函数。

**说明：** 部分geometry类型（如POLYHEDRALSURFACE）目前不支持ST\_Within操作。

## 示例

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

