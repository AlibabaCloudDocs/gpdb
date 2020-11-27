# ST\_\{T\|2D\|2DT\|3D\|3DT\}Contains

ST\_ndContains函数，判断第一个参数所对应的对象在指定的坐标轴上是否包含第二个参数所对应的对象。

## 语法

```
bool ST_TContains(tsrange r, trajectory traj);
bool ST_TContains(trajectory traj, tsrange r);
bool ST_2DContains(geometry geom, trajectory traj);
bool ST_2DContains(trajectory traj, geometry geom);
bool ST_2DContains(geometry geom, trajectory traj, timestamp ts, timestamp te);
bool ST_2DContains(trajectory traj, geometry geom, timestamp ts, timestamp te);
bool ST_{2D|2DT|3D|3DT}Contains(boxndf box, trajectory traj);
bool ST_{2D|2DT|3D|3DT}Contains(boxndf box, trajectory traj, timestamp ts, timestamp te);
```

## 参数

|参数名称|描述|
|----|--|
|geom|需要判断的几何对象。|
|traj|需要判断的轨迹对象，或者产生子轨迹的原始轨迹。|
|box|需要判断的外包框对象。|
|r|需要判读的时间范围。|
|ts|如存在，表示按此时间作为开始时间截取子轨迹。|
|te|如存在，表示按此时间作为结束时间截取子轨迹。|

## 描述

判断第一个参数是否包含第二个参数。

-   对于geometry类型，目前仅支持二维的操作，即判断轨迹或一定时间段内的子轨迹的二维投影，是否包含或被包含于给定的geom内。
-   对于boxndf作为第一个参数，trajectory作为第二个参数的Contains函数，将判断各个维度轨迹（或子轨迹）在指定的维度上是否在给定的外包框范围内。如果外包框、轨迹或子轨迹不包含某个给定的维度，则此维度将取任意值，即对此坐标轴自动满足contains条件。

**说明：** 部分geometry类型（如POLYHEDRALSURFACE）目前不支持ST\_Contains操作。

## 示例

```
WITH traj AS(
    Select ST_makeTrajectory('STPOINT', 'LINESTRING(0 0, 50 50, 100 100)'::geometry,
                             ('[' || ST_PGEpochToTS(0) || ',' || ST_PGEpochToTS(100) || ')')::tsrange,
                             '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120,130,140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120,130,140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120,130,140]}, "acceleration": {"type": "string", "length": 20, "nullable" : true,"value": ["120","130","140"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 11:35:00 2010", "Fri Jan 01 12:35:00 2010", "Fri Jan 01 13:30:00 2010"]}}, "events": [{"2" : "Fri Jan 02 15:00:00 2010"}, {"3" : "Fri Jan 02 15:30:00 2010"}]}') b,
           ST_MakeBox3dt(0,0,0,ST_PgEpochToTS(0), 50,50,50, ST_PgEpochToTS(49)) a
)
SELECT ST_2dContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(49)), ST_3dContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(49)),
       ST_2dtContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(49)), ST_3dtContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(49)),
       ST_2dContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(50)), ST_3dContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(50)),
       ST_2dtContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(50)), ST_3dtContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(50)),
       ST_2dContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(70)), ST_3dContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(70)),
       ST_2dtContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(70)), ST_3dtContains(a,b,ST_PGEpochToTS(0), ST_PGEpochToTS(70)) from traj;
 st_2dcontains | st_3dcontains | st_2dtcontains | st_3dtcontains | st_2dcontains | st_3dcontains | st_2dtcontains | st_3dtcontains | st_2dcontains | st_3dcontains | st_2dtcontains | st_3dtcontains 
---------------+---------------+----------------+----------------+---------------+---------------+----------------+----------------+---------------+---------------+----------------+----------------
 t             | t             | t              | t              | t             | t             | f              | f              | f             | f             | f              | f
```

