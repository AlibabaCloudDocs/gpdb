# JSON 数据类型操作 {#concept_osb_pxy_52b .concept}

JSON 类型几乎已成为互联网及物联网（IoT）的基础数据类型，具体协议请参见 [JSON 官网](http://www.json.org/)。AnalyticDB for PostgreSQL数据库对JSON数据类型做了完善的支持。这部分介绍对JSON数据类型的操作，包括：

-   [JSON输入输出语法](#section_g2k_mpc_s3b)
-   [JSON操作符](#section_u26_huu_nta)
-   [JSON创建函数](#section_aib_fkj_800)
-   [JSON处理函数](#section_o8w_ie1_pj8)

## JSON输入输出语法 {#section_g2k_mpc_s3b .section}

AnalyticDB for PostgreSQL支持的JSON数据类型的输入和输出语法详见[RFC 7159](https://tools.ietf.org/html/rfc7159#page-4)。

一个JSON数值可以是一个简单值（数字、字符串、true/null/false），数组，对象。下列都是合法的json表达式：

``` {#codeblock_iqi_qx4_nrs}
-- 简单标量/简单值
-- 简单值可以是数字、带引号的字符串、true、false或者null
SELECT '5'::json;

-- 零个或者更多个元素的数组（元素类型可以不同）
SELECT '[1, 2, "foo", null]'::json;

-- 含有键/值对的对象
-- 注意对象的键必须总是带引号的字符串
SELECT '{"bar": "baz", "balance": 7.77, "active": false}'::json;

-- 数组和对象可以任意嵌套
SELECT '{"foo": [true, "bar"], "tags": {"a": 1, "b": null}}'::json;
```

## JSON操作符 {#section_u26_huu_nta .section}

下表说明了可以用于JSON数据类型的操作符。

|操作符|右操作数类型|描述|例子|结果|
|---|------|--|--|--|
|-\>|int|获得JSON数组元素（索引从零开始）。|`'[{"a":"foo"}, {"b":"bar"}, {"c":"baz"}]'::json->2`|`{"c":"baz"}`|
|-\>|text|根据键获得JSON对象的域。|`'{"a": {"b":"foo"}}'::json->'a'`|`{"b":"foo"}`|
|-\>\>|int|获得JSON数组元素的文本形式。|`'[1,2,3]'::json->>2`|`3`|
|-\>\>|text|获得JSON对象域的文本形式。|`'{"a":1,"b":2}'::json->>'b'`|`2`|
|\#\>|text\[\]|获得在指定路径上的JSON对象。|`'{"a": {"b":{"c": "foo"}}}'::json#>'{a,b}'`|`{"c": "foo"}`|
|\#\>\>|text\[\]|获得在指定路径上的JSON对象的文本形式。|`'{"a":[1,2,3], "b":[4,5,6]}'::json#>>'{a,2}'`|`3`|

## JSON创建函数 {#section_aib_fkj_800 .section}

下表说明了用于创建JSON值的函数。

|函数|描述|例子|结果|
|--|--|--|--|
|`to_json (anyelement)`|返回该值作为一个合法的JSON对象。数组和组合会被递归处理并且转换成数组和对象。如果输入包含一个从该类型到JSON的造型，会使用该cast函数来执行转换，否则将会产生一个JSON标量值。对于任何非数字、布尔值或空值的标量类型，会使用其文本表示，并且加上适当的引号和转义让它变成一个合法的JSON字符串。|`to_json ('Fred said "Hi."'::text)`|`"Fred said \"Hi.\""`|
|`array_to_json (anyarray [, pretty_bool])`|返回该数组为一个JSON数组。一个多维数组会变成一个JSON数组的数组。 **说明：** 如果pretty\_bool为true，在第一维元素之间会增加换行。

 |`array_to_json ('{{1,5},{99,100}}'::int[])`|`[[1,5],[99,100]]`|
|`row_to_json (record [, pretty_bool])`|返回该行为一个JSON对象。 **说明：** 如果pretty\_bool为true，在第一级别元素之间会增加换行。

 |`row_to_json (row(1,'foo'))`|`{"f1":1,"f2":"foo"}`|

## JSON处理函数 {#section_o8w_ie1_pj8 .section}

下表说明了处理JSON值的函数。

|函数|返回类型|描述|例子|例子结果|
|--|----|--|--|----|
|`json_each(json)`|`setof key text, value json setof key text, value jsonb`|把最外层的JSON对象展开成键/值对的集合。|`select * from json_each('{"a":"foo", "b":"bar"}')`| ``` {#codeblock_2h7_ywm_1e9}
 key | value
-----+-------
 a   | "foo"
 b   | "bar"
```

 |
|`json_each_text(json)`|`setof key text, value text`|把最外层的JSON对象展开成键/值对的集合。返回值的类型是text。|`select * from json_each_text('{"a":"foo", "b":"bar"}')`| ``` {#codeblock_dmd_ci4_exh}
 key | value
-----+-------
 a   | foo
 b   | bar
```

 |
|`json_extract_path(from_json json, VARIADIC path_elems text[])`|`json`|返回path\_elems指定的JSON值。等效于\#\>操作符。|`json_extract_path('{"f2":{"f3":1},"f4":{"f5":99,"f6":"foo"}}','f4')`| ``` {#codeblock_kce_czp_h7u}
{"f5":99,"f6":"foo"}
```

 |
|`json_extract_path_text(from_json json, VARIADIC path_elems text[])`|`text`|返回path\_elems指定的JSON值为文本。等效于\#\>\>操作符。|`json_extract_path_text('{"f2":{"f3":1},"f4":{"f5":99,"f6":"foo"}}','f4', 'f6')`| ``` {#codeblock_wmt_7f1_mmj}
foo
```

 |
|`json_object_keys(json)`|`setof text`|返回最外层JSON对象中的键集合。|`json_object_keys('{"f1":"abc","f2":{"f3":"a", "f4":"b"}}')`| ``` {#codeblock_9m6_kef_1tu}
 json_object_keys
------------------
 f1
 f2
```

 |
|`json_populate_record(base anyelement, from_json json)`|`anyelement`|把Expands the object in from\_json中的对象展开成一行，其中的列匹配由base定义的记录类型。|`select * from json_populate_record(null::myrowtype, '{"a":1,"b":2}')`| ``` {#codeblock_lmy_inz_uaq}
 a | b
---+---
 1 | 2
```

 |
|`json_populate_recordset(base anyelement, from_json json)`|`setof anyelement`|将from\_json中最外层的对象数组展开成一个行集合，其中的列匹配由base定义的记录类型。|`select * from json_populate_recordset(null::myrowtype, '[{"a":1,"b":2},{"a":3,"b":4}]')`| ``` {#codeblock_xww_bti_r4b}
 a | b
---+---
 1 | 2
 3 | 4
```

 |
|`json_array_elements(json)`|`setof json`|将一个JSON数组展开成JSON值的一个集合。|`select * from json_array_elements('[1,true, [2,false]]')`| ``` {#codeblock_fv6_8ch_zkn}
   value
-----------
 1
 true
 [2,false]
```

 |

## JSON操作举例 {#section_2k2_5k5_0v0 .section}

**创建表**

``` {#codeblock_29e_oca_c2u}
create table tj(id serial, ary int[], obj json, num integer);
=> insert into tj(ary, obj, num) values('{1,5}'::int[], '{"obj":1}', 5);
INSERT 0 1
=> select row_to_json(q) from (select id, ary, obj, num from tj) as q;
                row_to_json
-------------------------------------------
 {"f1":1,"f2":[1,5],"f3":{"obj":1},"f4":5}
(1 row)
=> insert into tj(ary, obj, num) values('{2,5}'::int[], '{"obj":2}', 5);
INSERT 0 1
=> select row_to_json(q) from (select id, ary, obj, num from tj) as q;
                row_to_json
-------------------------------------------
 {"f1":1,"f2":[1,5],"f3":{"obj":1},"f4":5}
 {"f1":2,"f2":[2,5],"f3":{"obj":2},"f4":5}
(2 rows)
```

**说明：** JSON 类型不能支持作为分布键来使用；也不支持 JSON 聚合函数。

**多表 JOIN**

``` {#codeblock_jtv_nl6_vhc}
create table tj2(id serial, ary int[], obj json, num integer);
=> insert into tj2(ary, obj, num) values('{2,5}'::int[], '{"obj":2}', 5);
INSERT 0 1
=> select * from tj, tj2 where tj.obj->>'obj' = tj2.obj->>'obj';
 id |  ary  |    obj    | num | id |  ary  |    obj    | num
----+-------+-----------+-----+----+-------+-----------+-----
  2 | {2,5} | {"obj":2} |   5 |  1 | {2,5} | {"obj":2} |   5
(1 row)
=> select * from tj, tj2 where json_object_field_text(tj.obj, 'obj') = json_object_field_text(tj2.obj, 'obj');
 id |  ary  |    obj    | num | id |  ary  |    obj    | num
----+-------+-----------+-----+----+-------+-----------+-----
  2 | {2,5} | {"obj":2} |   5 |  1 | {2,5} | {"obj":2} |   5
(1 row)
```

**JSON 函数索引**

``` {#codeblock_7pz_xqb_hkj}
CREATE TEMP TABLE test_json (
       json_type text,
       obj json
);
=> insert into test_json values('aa', '{"f2":{"f3":1},"f4":{"f5":99,"f6":"foo"}}');
INSERT 0 1
=> insert into test_json values('cc', '{"f7":{"f3":1},"f8":{"f5":99,"f6":"foo"}}');
INSERT 0 1
=> select obj->'f2' from test_json where json_type = 'aa';
 ?column?
----------
 {"f3":1}
(1 row)
=> create index i on test_json (json_extract_path_text(obj, '{f4}'));
CREATE INDEX
=> select * from test_json where json_extract_path_text(obj, '{f4}') = '{"f5":99,"f6":"foo"}';
 json_type |                 obj
-----------+-------------------------------------------
 aa        | {"f2":{"f3":1},"f4":{"f5":99,"f6":"foo"}}
(1 row)
```

下面是 Python 访问的一个例子：

``` {#codeblock_bxg_hgc_ann}
#! /bin/env python
import time
import json
import psycopg2
def gpquery(sql):
    conn = None
    try:
        conn = psycopg2.connect("dbname=sanity1x2")
        conn.autocommit = True
        cur = conn.cursor()
        cur.execute(sql)
        return cur.fetchall()
    except Exception as e:
        if conn:
            try:
                conn.close()
            except:
                pass
            time.sleep(10)
        print e
    return None
def main():
    sql = "select obj from tj;"
    #rows = Connection(host, port, user, pwd, dbname).query(sql)
    rows = gpquery(sql)
    for row in rows:
        print json.loads(row[0])
if __name__ == "__main__":
    main()
```

