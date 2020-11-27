# ST\_\{2D\|2DT\|3D\|3DT\}DWithin\_IndexLeft

ST\_ndDwithin\_IndexLeft函数，判断指定的两个对象在指定的坐标轴上是否距离小于等于某一给定值，并指定使用第一个参数所在的列的索引。

## 语法

```
bool ST_{2D|2DT|3D|3DT}DWithin_IndexLeft(trajectory traj1, trajectory traj2, float8 dist);
bool ST_{2D|2DT|3D|3DT}DWithin_IndexLeft(trajectory traj1, trajectory traj2, timestamp ts, timestamp te, float8 dist);
```

## 参数

|参数名称|描述|
|----|--|
|traj1|需要判断的轨迹对象，或者产生子轨迹的原始轨迹。|
|traj2|需要判断的轨迹对象，或者产生子轨迹的原始轨迹。|
|ts|如存在，表示按此时间作为开始时间截取子轨迹。|
|te|如存在，表示按此时间作为结束时间截取子轨迹。|
|dist|距离的临界值。|

## 描述

当`ST_ndDWithin(...)`操作涉及两个轨迹列时（即调用`ST_{2D|2DT|3D|3DT}DWithin(trajectory traj1, trajectory traj2, float8 dist)`或`ST_{2D|2DT|3D|3DT}DWithin(trajectory traj1, trajectory traj2, timestamp ts, timestamp te, float8 dist)`函数时），数据库无法判断应当使用哪个列的索引，因此会带来额外的计算开销。

我们可以使用ST\_ndDwithin\_IndexLeft函数手工指定使用第一个参数所对应的列上的索引，以降低计算开销。

## 示例

```
EXPLAIN SELECT count(*) FROM sqltr_test_trajs WHERE ST_3DTDWithin_IndexLeft(traj, ST_makeTrajectory('STPOINT', 'LINESTRING(-70 -30, -72 -34, -73 -35)'::geometry, '[2000-01-01 00:01:30, 2000-01-01 00:15:00)'::tsrange,
  '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120,130,140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120,130,140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120,130,140]}, "acceleration": {"type": "string", "length": 20, "nullable" : true,"value": ["120","130","140"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 11:35:00 2010", "Fri Jan 01 12:35:00 2010", "Fri Jan 01 13:30:00 2010"]}}, "events": [{"2" : "Fri Jan 02 15:00:00 2010"}, {"3" : "Fri Jan 02 15:30:00 2010"}]}'), 2)；
Aggregate  (cost=4341.89..4341.90 rows=1 width=8)
   ->  Bitmap Heap Scan on sqltr_test_trajs  (cost=103.31..4340.56 rows=529 width=0)
         Recheck Cond: ('BOX2DT(-75 -37 2000-01-01 00:01:29.999994,-68 -28 2000-01-01 00:15:00)'::boxndf &/#& traj)
         Filter: _st_3dtdwithin(traj, '{"trajectory":{"version":1,"type":"STPOINT","leafcount":3,"start_time":"2000-01-01 00:01:30","end_time":"2000-01-01 00:15:00","spatial":"LINESTRING(-70 -30,-72 -34,-73 -35)","timeline":["2000-01-01 00:01:30","2000-01-01 00:08:15","2000-01-01 00:15:00"],"attributes":{"leafcount":3,"velocity":{"type":"integer","length":2,"nullable":true,"value":[120,130,140]},"accuracy":{"type":"float","length":4,"nullable":false,"value":[120.0,130.0,140.0]},"bearing":{"type":"float","length":8,"nullable":false,"value":[120.0,130.0,140.0]},"acceleration":{"type":"string","length":20,"nullable":true,"value":["120","130","140"]},"active":{"type":"timestamp","length":8,"nullable":false,"value":["2010-01-01 11:35:00","2010-01-01 12:35:00","2010-01-01 13:30:00"]}},"events":[{"2":"2010-01-02 15:00:00"},{"3":"2010-01-02 15:30:00"}]}}'::trajectory, '2'::double precision)
         ->  Bitmap Index Scan on sqltr_test_traj_gist_2dt  (cost=0.00..103.18 rows=1586 width=0)
               Index Cond: (traj &/#& 'BOX2DT(-75 -37 2000-01-01 00:01:29.999994,-68 -28 2000-01-01 00:15:00)'::boxndf)
```

