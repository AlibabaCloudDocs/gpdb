# ST\_attrDeduplicate

This function removes trajectory points whose values for the specified attribute field are duplicate from a trajectory object and returns the trajectory object after deduplication. The start and end trajectory points of the trajectory object must be kept.

## Syntax

```
trajectory ST_attrDeduplicate(trajectory traj, cstring attr_field_name);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|traj|The trajectory object.|
|attr\_field\_name|The name of the specified attribute field.|

## Examples

```
select st_attrDeduplicate(ST_makeTrajectory('STPOINT'::leaftype, 'LINESTRING(-179.48077 51.72814,-179.46731 51.74634,-179.46502 51.74934,-179.46183 51.75378,-179.45943 51.75736,-179.45560 51.76273,-179.44845 51.77186,-179.43419 51.78977,-179.41259 51.81643,-179.41001 51.81941,-179.40751 51.82223,-179.40497 51.82505,-179.40242 51.82796,-179.39981 51.83095,-179.39734 51.83398,-179.39499 51.83709)'::geometry, ARRAY['2017-01-15 09:06:39'::timestamp,'2017-01-15 09:13:39','2017-01-15 09:14:48','2017-01-15 09:16:28','2017-01-15 09:17:48','2017-01-15 09:19:48','2017-01-15 09:23:19','2017-01-15 09:30:28','2017-01-15 09:34:40','2017-01-15 09:36:59','2017-01-15 09:38:09','2017-01-15 09:39:18','2017-01-15 09:40:40','2017-01-15 09:47:38','2017-01-15 09:48:49','2017-01-15 21:18:30'], '{"leafcount": 16, "attributes": {"heading": {"type": "float", "length": 4, "nullable": false,"value": [23.0,23.0,23.0,23.0,21.0,21.0,72.0,72.0,72.0,72.0,73.0,74.0,73.0,73.0,73.0,73.0]}}}'),'heading') as traj;
                                                                                                                                                                                         
                                                     traj                                                                                                                                                                                
-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
 {"trajectory":{"version":1,"type":"STPOINT","leafcount":6,"start_time":"2017-01-15 09:17:48","end_time":"2017-01-15 21:18:30","spatial":"LINESTRING(-179.45943 51.75736,-179.44845 51.77
186,-179.40751 51.82223,-179.40497 51.82505,-179.39499 51.83709)","timeline":["2017-01-15 09:17:48","2017-01-15 09:23:19","2017-01-15 09:38:09","2017-01-15 09:39:18","2017-01-15 21:18:3
0"],"attributes":{"leafcount":6,"heading":{"type":"float","length":4,"nullable":false,"value":[23.0,21.0,72.0,73.0,74.0,73.0]}}}}
(1 row)
```

