# ST\_attrTimestampFilter

指定轨迹属性字段，根据固定值，过滤出符合条件的轨迹点，返回过滤后的属性字段值（数组）。

## 语法

```
timestamp[] ST_attrTimestampFilter(trajectory traj, cstring attr_field_name,cstring operator, timestamp value);
timestamp[] ST_attrTimestampFilter(trajectory traj, cstring attr_field_name,cstring operator, timestamp value1, timestamp value2);
```

## 参数

|参数名称|描述|
|----|--|
|traj|轨迹对象。|
|attr\_field\_name|指定的属性名称。|
|operator|过滤符，`'='`，`'!='`，`'>'`，`'<'`，`'>='`，`'<='`，`'[]'`，`'(]'`，`'[)'`，`'()'`。|
|value、value1|属性固定值下限。|
|value2|属性固定值上限。|

## 示例

```
insert into traj values(3,ST_makeTrajectory('STPOINT'::leaftype, st_geomfromtext('LINESTRING (114 35, 115 36, 116 37)', 4326), ARRAY['2010-01-01 14:30'::timestamp, '2010-01-01 15:00', '2010-01-01 15:30'], '{"leafcount": 3, "attributes" : {"heading" : {"type": "timestamp", "nullable" : false,"value":["Fri Jan 01 14:30:00 2010", "Fri Jan 01 15:00:00 2010", "Fri Jan 01 15:30:00 2010"]}}}'));
select st_attrTimestampFilter(traj, 'heading', '>', 'Fri Jan 01 15:00:00 2010'::timestamp) from traj where id = 3;
 st_attrtimestampfilter  
------------------------- 
{"2010-01-01 15:30:00"}
(1 row)
```

