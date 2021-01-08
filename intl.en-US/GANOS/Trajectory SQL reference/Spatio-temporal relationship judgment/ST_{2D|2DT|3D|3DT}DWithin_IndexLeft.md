# ST\_\{2D\|2DT\|3D\|3DT\}DWithin\_IndexLeft

This function is used to check whether the distance between two objects on a specified axis is less than or equal to a specified threshold. This function also allows you to specify to use the index on the column that stores the first specified object.

## Syntax

```
bool ST_{2D|2DT|3D|3DT}DWithin_IndexLeft(trajectory traj1, trajectory traj2, float8 dist);
bool ST_{2D|2DT|3D|3DT}DWithin_IndexLeft(trajectory traj1, trajectory traj2, timestamp ts, timestamp te, float8 dist);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj1|The trajectory that you want to compare, or the original trajectory that includes the sub-trajectory that you want to compare.|
|traj2|The trajectory that you want to compare, or the original trajectory that includes the sub-trajectory that you want to compare.|
|ts|The beginning of the time range over which you want to extract sub-trajectories. This parameter is optional.|
|te|The end of the time range over which you want to extract sub-trajectories. This parameter is optional.|
|dist|The threshold for the distance between the two objects.|

## Description

If you call the `ST_ndDWithin(...)` function to compare two trajectories by using the `ST_{2D|2DT|3D|3DT}DWithin(trajectory traj1, trajectory traj2, float8 dist)` or `ST_{2D|2DT|3D|3DT}DWithin(trajectory traj1, trajectory traj2, timestamp ts, timestamp te, float8 dist)` syntax, the function cannot determine which index to use. This causes additional computing overheads.

We recommend that you use the ST\_ndDwithin\_IndexLeft function. This function allows you to manually specify to use the index on the column that stores the first specified object. This reduces computing overheads.

## Example

```
EXPLAIN SELECT count(*) FROM sqltr_test_trajs WHERE ST_3DTDWithin_IndexLeft(traj, ST_makeTrajectory('STPOINT', 'LINESTRING(-70 -30, -72 -34, -73 -35)'::geometry, '[2000-01-01 00:01:30, 2000-01-01 00:15:00)'::tsrange,
  '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120,130,140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120,130,140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120,130,140]}, "acceleration": {"type": "string", "length": 20, "nullable" : true,"value": ["120","130","140"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 11:35:00 2010", "Fri Jan 01 12:35:00 2010", "Fri Jan 01 13:30:00 2010"]}}, "events": [{"2" : "Fri Jan 02 15:00:00 2010"}, {"3" : "Fri Jan 02 15:30:00 2010"}]}'), 2);
Aggregate  (cost=4341.89..4341.90 rows=1 width=8)
   ->  Bitmap Heap Scan on sqltr_test_trajs  (cost=103.31..4340.56 rows=529 width=0)
         Recheck Cond: ('BOX2DT(-75 -37 2000-01-01 00:01:29.999994,-68 -28 2000-01-01 00:15:00)'::boxndf &/#& traj)
         Filter: _st_3dtdwithin(traj, '{"trajectory":{"version":1,"type":"STPOINT","leafcount":3,"start_time":"2000-01-01 00:01:30","end_time":"2000-01-01 00:15:00","spatial":"LINESTRING(-70 -30,-72 -34,-73 -35)","timeline":["2000-01-01 00:01:30","2000-01-01 00:08:15","2000-01-01 00:15:00"],"attributes":{"leafcount":3,"velocity":{"type":"integer","length":2,"nullable":true,"value":[120,130,140]},"accuracy":{"type":"float","length":4,"nullable":false,"value":[120.0,130.0,140.0]},"bearing":{"type":"float","length":8,"nullable":false,"value":[120.0,130.0,140.0]},"acceleration":{"type":"string","length":20,"nullable":true,"value":["120","130","140"]},"active":{"type":"timestamp","length":8,"nullable":false,"value":["2010-01-01 11:35:00","2010-01-01 12:35:00","2010-01-01 13:30:00"]}},"events":[{"2":"2010-01-02 15:00:00"},{"3":"2010-01-02 15:30:00"}]}}'::trajectory, '2'::double precision)
         ->  Bitmap Index Scan on sqltr_test_traj_gist_2dt  (cost=0.00..103.18 rows=1586 width=0)
               Index Cond: (traj &/#& 'BOX2DT(-75 -37 2000-01-01 00:01:29.999994,-68 -28 2000-01-01 00:15:00)'::boxndf)
```

