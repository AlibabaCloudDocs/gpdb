# ST\_\{2D\|2DT\|3D\|3DT\}DWithin

ST\_ndDWithin函数，判断指定的两个对象在指定的坐标轴上是否距离小于等于某一给定值。

## 语法

```
bool ST_{2D|3D}DWithin(geometry geom, trajectory traj, float8 dist);
bool ST_{2D|3D}DWithin(trajectory traj, geometry geom, float8 dist);
bool ST_{2D|3D}DWithin(geometry geom, trajectory traj, timestamp ts, timestamp te, float8 dist);
bool ST_{2D|3D}DWithin(trajectory traj, geometry geom, timestamp ts, timestamp te, float8 dist);
bool ST_{2D|2DT|3D|3DT}DWithin(boxndf box, trajectory traj, float8 dist);
bool ST_{2D|2DT|3D|3DT}DWithin(trajectory traj, boxndf box, float8 dist);
bool ST_{2D|2DT|3D|3DT}DWithin(boxndf box, trajectory traj, timestamp ts, timestamp te, float8 dist);
bool ST_{2D|2DT|3D|3DT}DWithin(trajectory traj, boxndf box, timestamp ts, timestamp te, float8 dist);
bool ST_{2D|2DT|3D|3DT}DWithin(trajectory traj1, trajectory traj2, float8 dist);
bool ST_{2D|2DT|3D|3DT}DWithin(trajectory traj1, trajectory traj2, timestamp ts, timestamp te, float8 dist);
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
|dist|距离的临界值。|

## 描述

-   对于2D和3D的ST\_ndDWithin操作，我们判断给定的两个对象（即第一个和第二个参数）在二维/三维空间上的投影（即对应的geometry类型）是否距离小于等于给定值。
-   对于2DT和3DT的ST\_ndDWithin操作，我们判断两者是否在某一时间点上在指定维度上的距离小于等于过给定值。

如果函数包含ts和te参数，代表判断ST\_ndDWithin的轨迹对象（traj，或traj1和traj2）是在\[ts,te\]时间段内的子轨迹；否则则表示需要判断的轨迹对象（traj，或traj1和traj2）是完整的轨迹。

直接调用`ST_DistanceWithin(..)`函数相当于调用`ST_2DDWithin(...)`（若前两个参数其中一个为几何对象）或`ST_3DTDWithin(...)`（若前两个参数均为轨迹对象）。

## 示例

```
WITH traj AS(
    Select ST_makeTrajectory('STPOINT', 'LINESTRING(0 0 10, 50 0 10, 100 0 10)'::geometry,
                             ('[' || ST_PGEpochToTS(0) || ',' || ST_PGEpochToTS(100) || ')')::tsrange,
                             '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120,130,140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120,130,140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120,130,140]}, "acceleration": {"type": "string", "length": 20, "nullable" : true,"value": ["120","130","140"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 11:35:00 2010", "Fri Jan 01 12:35:00 2010", "Fri Jan 01 13:30:00 2010"]}}, "events": [{"2" : "Fri Jan 02 15:00:00 2010"}, {"3" : "Fri Jan 02 15:30:00 2010"}]}') a,
           ST_makeTrajectory('STPOINT', 'LINESTRING(0 20, 50 20, 100 20)'::geometry,
                             ('[' || ST_PGEpochToTS(0) || ',' || ST_PGEpochToTS(100) ||')')::tsrange,
                             '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120,130,140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120,130,140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120,130,140]}, "acceleration": {"type": "string", "length": 20, "nullable" : true,"value": ["120","130","140"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 11:35:00 2010", "Fri Jan 01 12:35:00 2010", "Fri Jan 01 13:30:00 2010"]}}, "events": [{"2" : "Fri Jan 02 15:00:00 2010"}, {"3" : "Fri Jan 02 15:30:00 2010"}]}') b
)
SELECT ST_2dDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(49), 19), ST_3dDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(49), 19),
       ST_2dDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(50), 20), ST_3dDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(50), 20),
       ST_2dDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(70), 21), ST_3dDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(70), 21),
       ST_2dtDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(49), 19), ST_3dtDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(49), 19),
       ST_2dtDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(50), 20), ST_3dtDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(50), 20),
       ST_2dtDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(70), 21), ST_3dtDWithin(a,b, ST_PGEpochToTS(0),ST_PGEpochToTS(70), 21) from traj;
NOTICE:  One or both of the geometries is missing z-value. The unknown z-value will be regarded as "any value"
NOTICE:  One or both of the geometries is missing z-value. The unknown z-value will be regarded as "any value"
 st_2ddwithin | st_3ddwithin | st_2ddwithin | st_3ddwithin | st_2ddwithin | st_3ddwithin | st_2dtdwithin | st_3dtdwithin | st_2dtdwithin | st_3dtdwithin | st_2dtdwithin | st_3dtdwithin 
--------------+--------------+--------------+--------------+--------------+--------------+---------------+---------------+---------------+---------------+---------------+---------------
 f            | f            | t            | t            | t            | t            | f             | f             | t             | t             | t             | t
```

