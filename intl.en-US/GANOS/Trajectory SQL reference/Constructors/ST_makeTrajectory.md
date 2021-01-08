# ST\_makeTrajectory

This function constructs a trajectory object.

## Syntax

```
trajectory ST_makeTrajectory (leaftype type, geometry spatial, tsrange timespan, cstring attrs_json);
trajectory ST_makeTrajectory (leaftype type, geometry spatial, timestamp start, timestamp end, cstring attrs_json);
trajectory ST_makeTrajectory (leaftype type, geometry spatial, timestamp[] timeline, cstring attrs_json);
trajectory ST_makeTrajectory (leaftype type, float8[] x, float8[] y, integer srid, timestamp[] timeline, text[] attr_field_names, int4[] attr_int4, float8[] attr_float8, text[] attr_cstring, anyarray attr_any);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|type|The leaf type of the trajectory. Only ST\_POINT is supported.|
|spatial|The spatial geometry object of the linestring or point type.|
|timespan|The time range of the trajectory. It contains a start time and an end time.|
|start|The start time of the trajectory.|
|end|The end time of the trajectory.|
|timeline|The timeline of the trajectory. The number of time points must be the same as that of points in the spatial geometry object of the linestring type.|
|attrs\_json|The attributes and events of the trajectory. The value must be in JSON format and can be null.|
|attr\_field\_names|The array of the names of all attribute fields for the trajectory.|
|x|The x-axis \(array\) for the spatial geometry object.|
|y|The y-axis \(array\) for the spatial geometry object.|
|srid|The spatial reference system identifier \(SRID\) of the trajectory. This parameter is required.|

1.  The following example shows the format of the attr\_json parameter:

    ```
    {
      "leafcount": 3,
      "attributes": {
        "velocity": {
          "type": "integer",
          "length": 2,
          "nullable": true,
          "value": [
            120,
            null,
            140
          ]
        },
        "accuracy": {
          "type": "float",
          "length": 4,
          "nullable": false,
          "value": [
            120,
            130,
            140
          ]
        },
        "bearing": {
          "type": "float",
          "length": 8,
          "nullable": false,
          "value": [
            120,
            130,
            140
          ]
        },
        "vesname": {
          "type": "string",
          "length": 20,
          "nullable": true,
          "value": [
            "dsff",
            "fgsd",
            null
          ]
        },
        "active": {
          "type": "timestamp",
          "nullable": false,
          "value": [
            "Fri Jan 01 14:30:00 2010",
            "Fri Jan 01 15:00:00 2010",
            "Fri Jan 01 15:30:00 2010"
          ]
        }
      },
      "events": [
        {
          "1": "Fri Jan 01 14:30:00 2010"
        },
        {
          "2": "Fri Jan 01 15:00:00 2010"
        },
        {
          "3": "Fri Jan 01 15:30:00 2010"
        }
      ]
    }
    ```

    **leafcount**: the number of trajectory points in the trajectory object. Its value must be consistent with the number of spatial points in the spatial geometry object. Its value is also the same as the number of values for each attribute field. All attribute fields must have the same number of values.

    **attributes**: the trajectory attributes, including the definitions and a sequence of values for all attribute fields. It must co-exist with the leafcount parameter. Attribute definitions and requirements are as follows:

    -   The name of an attribute field can contain a maximum of **60** characters.
    -   type: the data type of the field. Valid values: integer, float, string, timestamp, and bool.
    -   length: the length of the field. The length varies with the data type. integer: 1, 2, 4, or 8. float: 4 or 8. string: customizable. If not specified, the default length is 64 and the maximum length is 253. The length value is the actual number of characters and excludes the end identifier. timestamp: If not specified, the default length is 8. bool: If not specified, the default length is 1.
    -   nullable: specifies whether the field can be null. Valid values: true and false. Default value: true.
    -   value: the sequence of field values, listed in JSON array format. If a value is null, null is displayed.
    **events**: the trajectory events. Multiple events are listed in JSON array format. Each array element is expressed in key:value format, where key indicates the event type ID and value indicates the event time.

2.  If only the timespan parameter or the start and end parameters are specified as time, such as in syntax 1 and syntax 2, this constructor can interpolate time points based on the number of spatial points to generate the trajectory timeline.
3.  If all the four types of syntax do not meet your requirements, you can customize the parameters following the first six fixed parameters to create a custom ST\_makeTrajectory constructor as follows:

    ```
    CREATE OR REPLACE FUNCTION _ST_MakeTrajectory(type leaftype, x float8[], y float8[], srid integer, timespan timestamp[],
        attrs_name cstring[], attr1 float8[], attr2 float4[], attr3 timestamp[])
        RETURNS trajectory
        AS '$libdir/libpg-trajectoryxx','sqltr_traj_make_all_array'
        LANGUAGE 'c' IMMUTABLE Parallel SAFE;
    ```


## Examples

```
-- (1) ST_MakeTrajectory with a timestamp range
select ST_MakeTrajectory('STPOINT'::leaftype, st_geomfromtext('LINESTRING (114 35, 115 36, 116 37)', 4326), '[2010-01-01 14:30, 2010-01-01 15:30)'::tsrange, '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120, 130, 140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120, 130, 140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120, 130, 140]}, "vesname": {"type": "string", "length": 20, "nullable" : true,"value": ["adsf", "sdf", "sdfff"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 14:30:00 2010", "Fri Jan 01 15:00:00 2010", "Fri Jan 01 15:30:00 2010"]}}, "events": [{"1":"Fri Jan 01 14:30:00 2010"}, {"2":"Fri Jan 01 15:00:00 2010"}, {"3":"Fri Jan 01 15:30:00 2010"}]}');                        st_maketrajectory         
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- 
 {"trajectory":{"version":1,"type":"STPOINT","leafcount":3,"start_time":"2010-01-01 14:30:00","end_time":"2010-01-01 15:30:00","spatial":"SRID=4326;LINESTRING(114 35,115 36,116 37)","timeline":["2010-01-01 14:30:00","2010-01-01 15:00:00","2010-01-01 15:30:00"],"attributes":{"leafcount":3,"velocity":{"type":"integer","length":2,"nullable":true,"value":[120,130,140]},"accuracy":{"type":"float","length":4,"nullable":false,"value":[120.0,130.0,140.0]},"bearing":{"type":"float","length":8,"nullable":false,"value":[120.0,130.0,140.0]},"vesname":{"type":"string","length":20,"nullable":true,"value":["adsf","sdf","sdfff"]},"active":{"type":"timestamp","length":8,"nullable":false,"value":["2010-01-01 14:30:00","2010-01-01 15:00:00","2010-01-01 15:30:00"]}},"events":[{"1":"2010-01-01 14:30:00"},{"2":"2010-01-01 15:00:00"},{"3":"2010-01-01 15:30:00"}]}}
(1 row)

-- (2) ST_MakeTrajectory with a start timestamp and an end timestamp
select ST_MakeTrajectory('STPOINT'::leaftype, st_geomfromtext('LINESTRING (114 35, 115 36, 116 37)', 4326), '2010-01-01 14:30'::timestamp, '2010-01-01 15:30'::timestamp, '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120, 130, 140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120, 130, 140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120, 130, 140]}, "vesname": {"type": "string", "length": 20, "nullable" : true,"value": ["adsf", "sdf", "sdfff"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 14:30:00 2010", "Fri Jan 01 15:00:00 2010", "Fri Jan 01 15:30:00 2010"]}}, "events": [{"1":"Fri Jan 01 14:30:00 2010"}, {"2":"Fri Jan 01 15:00:00 2010"}, {"3":"Fri Jan 01 15:30:00 2010"}]}');                        st_maketrajectory         
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- 
 {"trajectory":{"version":1,"type":"STPOINT","leafcount":3,"start_time":"2010-01-01 14:30:00","end_time":"2010-01-01 15:30:00","spatial":"SRID=4326;LINESTRING(114 35,115 36,116 37)","timeline":["2010-01-01 14:30:00","2010-01-01 15:00:00","2010-01-01 15:30:00"],"attributes":{"leafcount":3,"velocity":{"type":"integer","length":2,"nullable":true,"value":[120,130,140]},"accuracy":{"type":"float","length":4,"nullable":false,"value":[120.0,130.0,140.0]},"bearing":{"type":"float","length":8,"nullable":false,"value":[120.0,130.0,140.0]},"vesname":{"type":"string","length":20,"nullable":true,"value":["adsf","sdf","sdfff"]},"active":{"type":"timestamp","length":8,"nullable":false,"value":["2010-01-01 14:30:00","2010-01-01 15:00:00","2010-01-01 15:30:00"]}},"events":[{"1":"2010-01-01 14:30:00"},{"2":"2010-01-01 15:00:00"},{"3":"2010-01-01 15:30:00"}]}}
(1 row)

-- (3) ST_MakeTrajectory with a timestamp array
select ST_MakeTrajectory('STPOINT'::leaftype, st_geomfromtext('LINESTRING (114 35, 115 36, 116 37)', 4326), ARRAY['2010-01-01 14:30'::timestamp, '2010-01-01 15:00'::timestamp, '2010-01-01 15:30'::timestamp], '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120, 130, 140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120, 130, 140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120, 130, 140]}, "vesname": {"type": "string", "length": 20, "nullable" : true,"value": ["adsf", "sdf", "sdfff"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 14:30:00 2010", "Fri Jan 01 15:00:00 2010", "Fri Jan 01 15:30:00 2010"]}}, "events": [{"1":"Fri Jan 01 14:30:00 2010"}, {"2":"Fri Jan 01 15:00:00 2010"}, {"3":"Fri Jan 01 15:30:00 2010"}]}');                        st_maketrajectory         
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- 
 {"trajectory":{"version":1,"type":"STPOINT","leafcount":3,"start_time":"2010-01-01 14:30:00","end_time":"2010-01-01 15:30:00","spatial":"SRID=4326;LINESTRING(114 35,115 36,116 37)","timeline":["2010-01-01 14:30:00","2010-01-01 15:00:00","2010-01-01 15:30:00"],"attributes":{"leafcount":3,"velocity":{"type":"integer","length":2,"nullable":true,"value":[120,130,140]},"accuracy":{"type":"float","length":4,"nullable":false,"value":[120.0,130.0,140.0]},"bearing":{"type":"float","length":8,"nullable":false,"value":[120.0,130.0,140.0]},"vesname":{"type":"string","length":20,"nullable":true,"value":["adsf","sdf","sdfff"]},"active":{"type":"timestamp","length":8,"nullable":false,"value":["2010-01-01 14:30:00","2010-01-01 15:00:00","2010-01-01 15:30:00"]}},"events":[{"1":"2010-01-01 14:30:00"},{"2":"2010-01-01 15:00:00"},{"3":"2010-01-01 15:30:00"}]}}
(1 row)  

-- (4) ST_MakeTrajectory with null attrs_json
select ST_MakeTrajectory('STPOINT'::leaftype, st_geomfromtext('LINESTRING (114 35, 115 36, 116 37)', 4326), '[2010-01-01 14:30, 2010-01-01 15:30)'::tsrange, null); 
                        st_maketrajectory                                                                                                                  
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ 
{"trajectory":{"leafsize":3,"starttime":"Fri Jan 01 14:30:00 2010","endtime":"Fri Jan 01 15:30:00 2010","spatial":"LINESTRING(114 35,115 36,116 37)","timeline":["Fri Jan 01 14:30:00 2010","Fri Jan 01 15:00:00 2010","Fri Jan 01 15:30:00 2010"]}}
(1 row)

-- (5) ST_MakeTrajectory from points
select st_makeTrajectory('STPOINT'::leaftype, ARRAY[1::float8], ARRAY[2::float8], 4326, ARRAY['2010-01-01 11:30'::timestamp], ARRAY['velocity'], ARRAY[1::int4], NULL, NULL, NULL::anyarray);
                                                            st_maketrajectory                             

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
{"trajectory":{"version":1,"type":"STPOINT","leafcount":1,"start_time":"2010-01-01 11:30:00","end_time":"2010-01-01 11:30:00","spatial":"SRID=4326;POINT(1 2)","timeline":["2010-01-01 11:30:00"],"attributes":{"leafcount":1,"velocity":{"type":"integer","length":4,"nullable":true,"value":[1]}}}}
(1 row)
```

