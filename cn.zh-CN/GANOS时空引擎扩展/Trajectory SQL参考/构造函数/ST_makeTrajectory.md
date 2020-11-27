# ST\_makeTrajectory

构造一个trajectory对象。

## 语法

```
trajectory  ST_makeTrajectory (leaftype type, geometry spatial, tsrange timespan , cstring attrs_json);
trajectory  ST_makeTrajectory (leaftype type, geometry spatial, timestamp start,  timestamp end ,  cstring attrs_json);
trajectory  ST_makeTrajectory (leaftype type, geometry spatial, timestamp[]  timeline, cstringattrs_json );
trajectory  ST_makeTrajectory (leaftype type, float8[] x, float8[] y, integer srid, timestamp[] timeline, text[] attr_field_names, int4[] attr_int4, float8[] attr_float8, text[] attr_cstring , anyarrayattr_any );  
```

## 参数

|参数名称|描述|
|----|--|
|type|轨迹的类型，目前只支持 ST\_POINT。|
|spatial|基于 LineString/Point类型的轨迹空间对象。|
|timespan|包含开始时间和结束时间的tsrange。|
|start|轨迹的开始时间。|
|end|轨迹的结束时间。|
|timeline|轨迹的时间序列，数量必须和linestring的点数量一致。|
|attrs\_json|轨迹属性和事件，JSON格式，可以为空值。|
|attr\_field\_names|表示轨迹属性的所有字段名称（数组）。|
|x|用于构建几何对象的x坐标（数组）。|
|y|用于构建几何对象的y坐标（数组）。|
|srid|轨迹空间参考。必须存在。|

1.  attr\_json的格式为：

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

    **leafcount**为轨迹包含的轨迹点个数，必须与spatial对象中包含的空间点个数一致，同时也是每个属性字段包含的元素值个数，所有属性元素个数都必须一致；

    **attributes**为属性项，包含所有属性的字段定义及值序列，与leafcount同时存在，属性定义及要求：

    -   属性名称最多**60**个字符；
    -   type为字段类型，支持integer，float，string, timestamp，bool五种数据类型；
    -   length为字段长度，integer支持长度为1、2、4、8; float支持长度为4、8；string可自定义长度，不指定时默认长度为64，最大长度为253，该长度值为字符实际个数，不包含末尾的结束标识；timestamp长度可不指定，默认为8；bool长度可不指定，默认为1;
    -   nullable为字段是否允许为空，true为允许为空，false不允许为空，默认值为true；
    -   value为字段值序列，用JSON数组表达，单个元素值为空用null表达。
    **events**为轨迹事件，用JSON数组表达多个事件，数组元素用JSON的“key:value”表达，key为事件类型，value为事件时间。

2.  如果传入的时间参数为 timespan 或者 start、end，则会根据spatial中点的个数进行插值。
3.  如果形式4不满足实际使用，可以自定义MakeTrajectory函数，前六个为固定参数，后面的参数可以根据实际情况进行定制：

    ```
    CREATE OR REPLACE FUNCTION  _ST_MakeTrajectory(type leaftype, x float8[], y float8[] , srid integer, timespan timestamp[],
        attrs_name cstring[], attr1 float8[], attr2 float4[], attr3 timestamp[])
        RETURNS trajectory
        AS '$libdir/libpg-trajectoryxx','sqltr_traj_make_all_array'
        LANGUAGE 'c' IMMUTABLE Parallel SAFE;
    ```


## 示例

```
-- (1) ST_MakeTrajectory with timestamp range
select ST_MakeTrajectory('STPOINT'::leaftype, st_geomfromtext('LINESTRING (114 35, 115 36, 116 37)', 4326), '[2010-01-01 14:30, 2010-01-01 15:30)'::tsrange, '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120, 130, 140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120, 130, 140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120, 130, 140]}, "vesname": {"type": "string", "length": 20, "nullable" : true,"value": ["adsf", "sdf", "sdfff"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 14:30:00 2010", "Fri Jan 01 15:00:00 2010", "Fri Jan 01 15:30:00 2010"]}}, "events": [{"1" : "Fri Jan 01 14:30:00 2010"}, {"2" : "Fri Jan 01 15:00:00 2010"}, {"3" : "Fri Jan 01 15:30:00 2010"}]}');                        st_maketrajectory         
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- 
 {"trajectory":{"version":1,"type":"STPOINT","leafcount":3,"start_time":"2010-01-01 14:30:00","end_time":"2010-01-01 15:30:00","spatial":"SRID=4326;LINESTRING(114 35,115 36,116 37)","timeline":["2010-01-01 14:30:00","2010-01-01 15:00:00","2010-01-01 15:30:00"],"attributes":{"leafcount":3,"velocity":{"type":"integer","length":2,"nullable":true,"value":[120,130,140]},"accuracy":{"type":"float","length":4,"nullable":false,"value":[120.0,130.0,140.0]},"bearing":{"type":"float","length":8,"nullable":false,"value":[120.0,130.0,140.0]},"vesname":{"type":"string","length":20,"nullable":true,"value":["adsf","sdf","sdfff"]},"active":{"type":"timestamp","length":8,"nullable":false,"value":["2010-01-01 14:30:00","2010-01-01 15:00:00","2010-01-01 15:30:00"]}},"events":[{"1":"2010-01-01 14:30:00"},{"2":"2010-01-01 15:00:00"},{"3":"2010-01-01 15:30:00"}]}}
(1 row)

-- (2) ST_MakeTrajectory with start timestamp and end timestamp
select ST_MakeTrajectory('STPOINT'::leaftype, st_geomfromtext('LINESTRING (114 35, 115 36, 116 37)', 4326), '2010-01-01 14:30'::timestamp, '2010-01-01 15:30'::timestamp, '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120, 130, 140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120, 130, 140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120, 130, 140]}, "vesname": {"type": "string", "length": 20, "nullable" : true,"value": ["adsf", "sdf", "sdfff"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 14:30:00 2010", "Fri Jan 01 15:00:00 2010", "Fri Jan 01 15:30:00 2010"]}}, "events": [{"1" : "Fri Jan 01 14:30:00 2010"}, {"2" : "Fri Jan 01 15:00:00 2010"}, {"3" : "Fri Jan 01 15:30:00 2010"}]}');                        st_maketrajectory         
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- 
 {"trajectory":{"version":1,"type":"STPOINT","leafcount":3,"start_time":"2010-01-01 14:30:00","end_time":"2010-01-01 15:30:00","spatial":"SRID=4326;LINESTRING(114 35,115 36,116 37)","timeline":["2010-01-01 14:30:00","2010-01-01 15:00:00","2010-01-01 15:30:00"],"attributes":{"leafcount":3,"velocity":{"type":"integer","length":2,"nullable":true,"value":[120,130,140]},"accuracy":{"type":"float","length":4,"nullable":false,"value":[120.0,130.0,140.0]},"bearing":{"type":"float","length":8,"nullable":false,"value":[120.0,130.0,140.0]},"vesname":{"type":"string","length":20,"nullable":true,"value":["adsf","sdf","sdfff"]},"active":{"type":"timestamp","length":8,"nullable":false,"value":["2010-01-01 14:30:00","2010-01-01 15:00:00","2010-01-01 15:30:00"]}},"events":[{"1":"2010-01-01 14:30:00"},{"2":"2010-01-01 15:00:00"},{"3":"2010-01-01 15:30:00"}]}}
(1 row)

-- (3) ST_MakeTrajectory with timestamp array
select ST_MakeTrajectory('STPOINT'::leaftype, st_geomfromtext('LINESTRING (114 35, 115 36, 116 37)', 4326), ARRAY['2010-01-01 14:30'::timestamp, '2010-01-01 15:00'::timestamp, '2010-01-01 15:30'::timestamp], '{"leafcount":3,"attributes":{"velocity": {"type": "integer", "length": 2,"nullable" : true,"value": [120, 130, 140]}, "accuracy": {"type": "float", "length": 4, "nullable" : false,"value": [120, 130, 140]}, "bearing": {"type": "float", "length": 8, "nullable" : false,"value": [120, 130, 140]}, "vesname": {"type": "string", "length": 20, "nullable" : true,"value": ["adsf", "sdf", "sdfff"]}, "active": {"type": "timestamp", "nullable" : false,"value": ["Fri Jan 01 14:30:00 2010", "Fri Jan 01 15:00:00 2010", "Fri Jan 01 15:30:00 2010"]}}, "events": [{"1" : "Fri Jan 01 14:30:00 2010"}, {"2" : "Fri Jan 01 15:00:00 2010"}, {"3" : "Fri Jan 01 15:30:00 2010"}]}');                        st_maketrajectory         
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- 
 {"trajectory":{"version":1,"type":"STPOINT","leafcount":3,"start_time":"2010-01-01 14:30:00","end_time":"2010-01-01 15:30:00","spatial":"SRID=4326;LINESTRING(114 35,115 36,116 37)","timeline":["2010-01-01 14:30:00","2010-01-01 15:00:00","2010-01-01 15:30:00"],"attributes":{"leafcount":3,"velocity":{"type":"integer","length":2,"nullable":true,"value":[120,130,140]},"accuracy":{"type":"float","length":4,"nullable":false,"value":[120.0,130.0,140.0]},"bearing":{"type":"float","length":8,"nullable":false,"value":[120.0,130.0,140.0]},"vesname":{"type":"string","length":20,"nullable":true,"value":["adsf","sdf","sdfff"]},"active":{"type":"timestamp","length":8,"nullable":false,"value":["2010-01-01 14:30:00","2010-01-01 15:00:00","2010-01-01 15:30:00"]}},"events":[{"1":"2010-01-01 14:30:00"},{"2":"2010-01-01 15:00:00"},{"3":"2010-01-01 15:30:00"}]}}
(1 row)  

-- (4) json is null
select ST_MakeTrajectory('STPOINT'::leaftype, st_geomfromtext('LINESTRING (114 35, 115 36, 116 37)', 4326), '[2010-01-01 14:30, 2010-01-01 15:30)'::tsrange, null); 
                        st_maketrajectory                                                                                                                  
------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ 
{"trajectory":{"leafsize":3,"starttime":"Fri Jan 01 14:30:00 2010","endtime":"Fri Jan 01 15:30:00 2010","spatial":"LINESTRING(114 35,115 36,116 37)","timeline":["Fri Jan 01 14:30:00 2010","Fri Jan 01 15:00:00 2010","Fri Jan 01 15:30:00 2010"]}}
(1 row)

-- (5) ST_MakeTrajectory make from points
select st_makeTrajectory('STPOINT'::leaftype, ARRAY[1::float8], ARRAY[2::float8], 4326, ARRAY['2010-01-01 11:30'::timestamp], ARRAY['velocity'], ARRAY[1::int4], NULL, NULL, NULL::anyarray);
                                                            st_maketrajectory                             

----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
{"trajectory":{"version":1,"type":"STPOINT","leafcount":1,"start_time":"2010-01-01 11:30:00","end_time":"2010-01-01 11:30:00","spatial":"SRID=4326;POINT(1 2)","timeline":["2010-01-01 11:30:00"],"attributes":{"leafcount":1,"velocity":{"type":"integer","length":4,"nullable":true,"value":[1]}}}}
(1 row)
```

