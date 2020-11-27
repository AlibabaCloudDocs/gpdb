# ST\_\{2D\|2DT\|3D\|3DT\}Intersects\_IndexLeft

ST\_ndIntersects\_IndexLeft函数，判断指定的两个对象在指定的坐标轴上是否相交，并指定使用第一个参数所在的列的索引。

## 语法

```
bool ST_{2D|2DT|3D|3DT}Intersects_IndexLeft(trajectory traj1, trajectory traj2, timestamp ts, timestamp te);
```

## 参数

|参数名称|描述|
|----|--|
|traj1|需要判断的轨迹对象，或者产生子轨迹的原始轨迹。|
|traj2|需要判断的轨迹对象，或者产生子轨迹的原始轨迹。|
|ts|如存在，表示按此时间作为开始时间截取子轨迹。|
|te|如存在，表示按此时间作为结束时间截取子轨迹。|

## 描述

当ST\_ndIntersects函数需要判断两条子轨迹是否相交时（即调用`ST_{2D|2DT|3D|3DT}Intersects(trajectory traj1, trajectory traj2, timestamp ts, timestamp te)`函数时），函数无法自主判断要使用两个轨迹列哪一列的索引，因此会带来额外的计算开销。

我们可以使用`ST_ndIntersects_IndexLeft(...)`函数，手工指定使用第一个参数所在的列上的索引，降低计算开销。

## 示例

```
EXPLAIN SELECT count(*) FROM sqltr_test_trajs WHERE ST_2DIntersects_IndexLeft(traj, ST_makeTrajectory('STPOINT', 'LINESTRING(-70 -30, -72 -34, -73 -35)'::geometry, '[2000-01-01 00:01:30, 2000-01-01 00:15:00)'::tsrange,
  '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120,130,140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120,130,140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120,130,140]}, "acceleration": {"type": "string", "length": 20, "nullable" : true,"value": ["120","130","140"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 11:35:00 2010", "Fri Jan 01 12:35:00 2010", "Fri Jan 01 13:30:00 2010"]}}, "events": [{"2" : "Fri Jan 02 15:00:00 2010"}, {"3" : "Fri Jan 02 15:30:00 2010"}]}'), ST_PGEpochToTS(0), ST_PGEpochToTs(7000))；
Aggregate  (cost=4201.04..4201.05 rows=1 width=8)
   ->  Bitmap Heap Scan on sqltr_test_trajs  (cost=98.64..4199.77 rows=505 width=0)
         Recheck Cond: ('BOX2DT(-73 -35 2000-01-01 00:01:29.999994,-70 -30 2000-01-01 00:15:00)'::boxndf &#& traj)
         Filter: _st_2dintersects(traj, '{"trajectory":{"version":1,"type":"STPOINT","leafcount":3,"start_time":"2000-01-01 00:01:30","end_time":"2000-01-01 00:15:00","spatial":"LINESTRING(-70 -30,-72 -34,-73 -35)","timeline":["2000-01-01 00:01:30","2000-01-01 00:08:15","2000-01-01 00:15:00"],"attributes":{"leafcount":3,"velocity":{"type":"integer","length":2,"nullable":true,"value":[120,130,140]},"accuracy":{"type":"float","length":4,"nullable":false,"value":[120.0,130.0,140.0]},"bearing":{"type":"float","length":8,"nullable":false,"value":[120.0,130.0,140.0]},"acceleration":{"type":"string","length":20,"nullable":true,"value":["120","130","140"]},"active":{"type":"timestamp","length":8,"nullable":false,"value":["2010-01-01 11:35:00","2010-01-01 12:35:00","2010-01-01 13:30:00"]}},"events":[{"2":"2010-01-02 15:00:00"},{"3":"2010-01-02 15:30:00"}]}}'::trajectory, '2000-01-01 00:00:00'::timestamp without time zone, '2000-01-01 01:56:40'::timestamp without time zone)
         ->  Bitmap Index Scan on sqltr_test_traj_gist_2dt  (cost=0.00..98.51 rows=1515 width=0)
               Index Cond: (traj &#& 'BOX2DT(-73 -35 2000-01-01 00:01:29.999994,-70 -30 2000-01-01 00:15:00)'::boxndf)  
```

