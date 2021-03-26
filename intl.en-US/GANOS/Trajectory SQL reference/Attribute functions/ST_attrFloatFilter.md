# ST\_attrFloatFilter

This function filters all trajectory points to obtain the trajectory points whose values for the specified attribute field meet a filter condition based on a fixed value or a value range and returns an array of the attribute field values of these points.

## Syntax

```
float8[] ST_attrFloatFilter(trajectory traj, cstring attr_field_name,cstring operator, float8 value);
float8[] ST_attrFloatFilter(trajectory traj, cstring attr_field_name,cstring operator, float8 value1, float8 value2);
```

## Parameter

|Option|Description|
|------|-----------|
|traj|The trajectory object.|
|attr\_field\_name|The name of the specified attribute field.|
|operator|Filter, `'='`, `'! ='`, `'>'`, `'<'`, `'>='`, `'<='`, `'[]'`, `'(]'`, `'[)'`, `'()'`.|
|value and value1|The fixed value of the attribute field and the minimum fixed value of the attribute field.|
|value2|The maximum fixed value of the attribute field.|

## Description

This function supports only float-type attribute fields.

## Examples

```
create table traj(id integer, traj trajectory);
insert into traj values(2,ST_makeTrajectory('STPOINT'::leaftype, 'LINESTRING(-179.48077 51.72814,-179.46731 51.74634,-179.46502 51.74934,-179.46183 51.75378,-179.45943 51.75736,-179.45560 51.76273,-179.44845 51.77186,-179.43419 51.78977,-179.41259 51.81643,-179.41001 51.81941,-179.40751 51.82223,-179.40497 51.82505,-179.40242 51.82796,-179.39981 51.83095,-179.39734 51.83398,-179.39499 51.83709)'::geometry, ARRAY['2017-01-15 09:06:39'::timestamp,'2017-01-15 09:14:48','2017-01-15 09:13:39','2017-01-15 09:16:28','2017-01-15 09:19:48','2017-01-15 09:17:48','2017-01-15 09:23:19','2017-01-15 09:34:40','2017-01-15 09:30:28','2017-01-15 09:36:59','2017-01-15 09:38:09','2017-01-15 09:39:18','2017-01-15 09:40:40','2017-01-15 09:47:38','2017-01-15 21:18:30','2017-01-15 09:48:49'], '{"leafcount": 16, "attributes": {"heading": {"type": "float", "length": 8, "nullable": false,"value":[23.0,23.0,23.0,23.0,21.0,21.0,72.0,72.0,72.0,72.0,73.0,74.0,73.0,73.0,73.0,73.0]}}}'));


select st_attrFloatFilter(traj, 'heading', '>', 23.0) from traj where id = 2;
       st_attrfloatfilter        
---------------------------------
 {72,72,72,72,73,74,73,73,73,73}
(1 row)
```

