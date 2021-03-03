# JSON & JSONB 数据类型操作

JSON 类型几乎已成为互联网及物联网（IoT）的基础数据类型，具体协议请参见 [JSON 官网](http://www.json.org/)。AnalyticDB for PostgreSQL数据库对JSON数据类型做了完善的支持。并且AnalyticDB for PostgreSQL 6.0版本支持JSONB类型。这部分介绍对JSON & JSONB数据类型的操作，包括：

-   [JSON & JSONB的异同](#section_kfx_uem_o8c)
-   [JSON输入输出语法](#section_g2k_mpc_s3b)
-   [JSON操作符](#section_u26_huu_nta)
-   [JSONB操作符](#section_in7_wik_di8)
-   [JSON创建函数](#section_aib_fkj_800)
-   [JSON处理函数](#section_o8w_ie1_pj8)
-   [JSONB创建索引](#section_nap_r06_41p)

## JSON & JSONB的异同

JSON和JSONB类型在使用上几乎完全一致，两者的区别主要在存储上，json数据类型直接存储输入文本的完全的拷贝，JSONB数据类型以二进制格式进行存储。同时JSONB相较于JSON更高效，处理速度提升非常大，且支持索引，一般情况下，AnalyticDB for PostgreSQL 6.0版本都建议使用JSONB类型替代JSON类型

## JSON输入输出语法

AnalyticDB for PostgreSQL支持的JSON数据类型的输入和输出语法详见[RFC 7159](https://tools.ietf.org/html/rfc7159#page-4)。

一个JSON数值可以是一个简单值（数字、字符串、true/null/false），数组，对象。下列都是合法的json表达式：

```
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

以上的JSON类型都可以写成JSONB类型的表达式，例如：

```
-- 简单标量/简单值,转化为jsonb类型
SELECT '5'::jsonb;
```

## JSON操作符

下表说明了可以用于JSON & JSONB数据类型的操作符。

|操作符|右操作数类型|描述|例子|结果|
|---|------|--|--|--|
|-\>|int|获得JSON数组元素（索引从零开始）。|`'[{"a":"foo"}, {"b":"bar"}, {"c":"baz"}]'::json->2`|`{"c":"baz"}`|
|-\>|text|根据键获得JSON对象的域。|`'{"a": {"b":"foo"}}'::json->'a'`|`{"b":"foo"}`|
|-\>\>|int|获得JSON数组元素的文本形式。|`'[1,2,3]'::json->>2`|`3`|
|-\>\>|text|获得JSON对象域的文本形式。|`'{"a":1,"b":2}'::json->>'b'`|`2`|
|\#\>|text\[\]|获得在指定路径上的JSON对象。|`'{"a": {"b":{"c": "foo"}}}'::json#>'{a,b}'`|`{"c": "foo"}`|
|\#\>\>|text\[\]|获得在指定路径上的JSON对象的文本形式。|`'{"a":[1,2,3], "b":[4,5,6]}'::json#>>'{a,2}'`|`3`|

下表说明了可以用于JSONB数据类型的操作符。

## JSONB操作符

下表说明了可以用于JSONB数据类型的操作符。

|操作符|右操作数类型|描述|例子|
|---|------|--|--|
|=|jsonb|两个JSON对象的内容是否相等|`'[1,2]'::jsonb= '[1,2]'::jsonb`|
|@\>|jsonb|左边的JSON对象是否包含右边的JSON对象|`'{"a":1, "b":2}'::jsonb @> '{"b":2}'::jsonb`|
|<@|jsonb|左边的JSON对象是否包含于右边的JSON对象|`'{"b":2}'::jsonb <@ '{"a":1, "b":2}'::jsonb`|
|?|text|指定的字符串是否存在与JSON对象中的key或者字符串类型的元素中|`'{"a":1, "b":2}'::jsonb ? 'b'`|
|?\||text\[\]|右值字符串数组是否存在任一元素在JSON对象字符串类型的key或者元素中|`'{"a":1, "b":2, "c":3}'::jsonb ?| array['b', 'c']`|
|?&|text\[\]|右值字符串数组是否所有元素在JSON对象字符串类型的key或者元素中|`'["a", "b"]'::jsonb ?& array['a', 'b']`|

## JSON创建函数

下表说明了用于创建JSON值的函数。

|函数|描述|例子|结果|
|--|--|--|--|
|`to_json (anyelement)`|返回该值作为一个合法的JSON对象。数组和组合会被递归处理并且转换成数组和对象。如果输入包含一个从该类型到JSON的造型，会使用该cast函数来执行转换，否则将会产生一个JSON标量值。对于任何非数字、布尔值或空值的标量类型，会使用其文本表示，并且加上适当的引号和转义让它变成一个合法的JSON字符串。|`to_json ('Fred said "Hi."'::text)`|`"Fred said \"Hi.\""`|
|`array_to_json (anyarray [, pretty_bool])`|返回该数组为一个JSON数组。一个多维数组会变成一个JSON数组的数组。 **说明：** 如果pretty\_bool为true，在第一维元素之间会增加换行。

|`array_to_json ('{{1,5},{99,100}}'::int[])`|`[[1,5],[99,100]]`|
|`row_to_json (record [, pretty_bool])`|返回该行为一个JSON对象。 **说明：** 如果pretty\_bool为true，在第一级别元素之间会增加换行。

|`row_to_json (row(1,'foo'))`|`{"f1":1,"f2":"foo"}`|

## JSON处理函数

下表说明了处理JSON值的函数。

|函数|返回类型|描述|例子|例子结果|
|--|----|--|--|----|
|`json_each(json)`|`set of key text, value json set of key text, value jsonb`|把最外层的JSON对象展开成键/值对的集合。|`select * from json_each('{"a":"foo", "b":"bar"}')`|```
 key | value
-----+-------
 a   | "foo"
 b   | "bar"
``` |
|`json_each_text(json)`|`set of key text, value text`|把最外层的JSON对象展开成键/值对的集合。返回值的类型是text。|`select * from json_each_text('{"a":"foo", "b":"bar"}')`|```
 key | value
-----+-------
 a   | foo
 b   | bar
``` |
|`json_extract_path(from_json json, VARIADIC path_elems text[])`|`json`|返回path\_elems指定的JSON值。等效于\#\>操作符。|`json_extract_path('{"f2":{"f3":1},"f4":{"f5":99,"f6":"foo"}}','f4')`|```
{"f5":99,"f6":"foo"}
``` |
|`json_extract_path_text(from_json json, VARIADIC path_elems text[])`|`text`|返回path\_elems指定的JSON值为文本。等效于\#\>\>操作符。|`json_extract_path_text('{"f2":{"f3":1},"f4":{"f5":99,"f6":"foo"}}','f4', 'f6')`|```
foo
``` |
|`json_object_keys(json)`|`setof text`|返回最外层JSON对象中的键集合。|`json_object_keys('{"f1":"abc","f2":{"f3":"a", "f4":"b"}}')`|```
 json_object_keys
------------------
 f1
 f2
``` |
|`json_populate_record(base anyelement, from_json json)`|`anyelement`|把Expands the object in from\_json中的对象展开成一行，其中的列匹配由base定义的记录类型。|`select * from json_populate_record(null::myrowtype, '{"a":1,"b":2}')`|```
 a | b
---+---
 1 | 2
``` |
|`json_populate_recordset(base anyelement, from_json json)`|`set of anyelement`|将from\_json中最外层的对象数组展开成一个行集合，其中的列匹配由base定义的记录类型。|`select * from json_populate_recordset(null::myrowtype, '[{"a":1,"b":2},{"a":3,"b":4}]')`|```
 a | b
---+---
 1 | 2
 3 | 4
``` |
|`json_array_elements(json)`|`set of json`|将一个JSON数组展开成JSON值的一个集合。|`select * from json_array_elements('[1,true, [2,false]]')`|```
   value
-----------
 1
 true
 [2,false]
``` |

## JSONB创建索引

JSONB类型支持GIN, BTree索引。一般情况下，我们会在JSONB类型字段上建GIN索引，语法如下：

```
CREATE INDEX idx_name ON table_name USING gin (idx_col);
CREATE INDEX idx_name ON table_name USING gin (idx_col jsonb_path_ops);
```

**说明：** 在JSONB上创建GIN索引的方式有两种：使用默认的jsonb\_ops操作符创建和使用jsonb\_path\_ops操作符创建。两者的区别是:在jsonb\_ops的GIN索引中，JSONB数据中的每个key和value都是作为一个单独的索引项的，而jsonb\_path\_ops则只为每个value创建一个索引项。

## JSON操作举例

创建表

```
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

多表 JOIN

```
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

JSON 函数索引

```
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

JSONB创建索引

```
-- 创建测试表并生成数据
CREATE TABLE jtest1 (
    id int,
    jdoc json
);

CREATE OR REPLACE FUNCTION random_string(INTEGER)
RETURNS TEXT AS
$BODY$
SELECT array_to_string(
    ARRAY (
        SELECT substring(
            '0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz'
            FROM (ceil(random()*62))::int FOR 1
        )
        FROM generate_series(1, $1)
    ),
    ''
)
$BODY$
LANGUAGE sql VOLATILE;

insert into jtest1 select t.seq, ('{"a":{"a1":"a1a1", "a2":"a2a2"}, 
"name":"'||random_string(10)||'","b":"bbbbb"}')::json from
generate_series(1, 10000000) as t(seq);

CREATE TABLE jtest2 (
    id int,
    jdoc jsonb
);

CREATE TABLE jtest3 (
    id int,
    jdoc jsonb
);

insert into jtest2 select id, jdoc::jsonb from jtest1;
insert into jtest3 select id, jdoc::jsonb from jtest1;

-- 建立索引
CREATE INDEX idx_jtest2 ON jtest2 USING gin(jdoc);
CREATE INDEX idx_jtest3 ON jtest3 USING gin(jdoc jsonb_path_ops);

-- 未建索引执行
EXPLAIN ANALYZE SELECT * FROM jtest1 where jdoc @> '{"name":"N9WP5txmVu"}';
                                                              QUERY PLAN
--------------------------------------------------------------------------------------------------------------------------------------
 Gather Motion 2:1  (slice1; segments: 2)  (cost=0.00..162065.73 rows=10100 width=88) (actual time=1343.248..1777.605 rows=1 loops=1)
   ->  Seq Scan on jtest2  (cost=0.00..162065.73 rows=5050 width=88) (actual time=0.042..1342.426 rows=1 loops=1)
         Filter: (jdoc @> '{"name": "N9WP5txmVu"}'::jsonb)
 Planning time: 0.172 ms
   (slice0)    Executor memory: 59K bytes.
   (slice1)    Executor memory: 91K bytes avg x 2 workers, 91K bytes max (seg0).
 Memory used:  2047000kB
 Optimizer: Postgres query optimizer
 Execution time: 1778.234 ms
(9 rows)

-- 使用jsonb_ops操作符创建索引执行
EXPLAIN ANALYZE SELECT * FROM jtest2 where jdoc @> '{"name":"N9WP5txmVu"}';
                                                           QUERY PLAN
--------------------------------------------------------------------------------------------------------------------------------
 Gather Motion 2:1  (slice1; segments: 2)  (cost=88.27..13517.81 rows=10100 width=88) (actual time=0.655..0.659 rows=1 loops=1)
   ->  Bitmap Heap Scan on jtest2  (cost=88.27..13517.81 rows=5050 width=88) (actual time=0.171..0.172 rows=1 loops=1)
         Recheck Cond: (jdoc @> '{"name": "N9WP5txmVu"}'::jsonb)
         ->  Bitmap Index Scan on idx_jtest2  (cost=0.00..85.75 rows=5050 width=0) (actual time=0.217..0.217 rows=1 loops=1)
               Index Cond: (jdoc @> '{"name": "N9WP5txmVu"}'::jsonb)
 Planning time: 0.151 ms
   (slice0)    Executor memory: 69K bytes.
   (slice1)    Executor memory: 628K bytes avg x 2 workers, 632K bytes max (seg1).  Work_mem: 9K bytes max.
 Memory used:  2047000kB
 Optimizer: Postgres query optimizer
 Execution time: 1.266 ms
(11 rows)

-- 使用jsonb_path_ops操作符创建索引执行
EXPLAIN ANALYZE SELECT * FROM jtest3 where jdoc @> '{"name":"N9WP5txmVu"}';
                                                           QUERY PLAN
--------------------------------------------------------------------------------------------------------------------------------
 Gather Motion 2:1  (slice1; segments: 2)  (cost=84.28..13513.81 rows=10101 width=88) (actual time=0.710..0.711 rows=1 loops=1)
   ->  Bitmap Heap Scan on jtest3  (cost=84.28..13513.81 rows=5051 width=88) (actual time=0.179..0.181 rows=1 loops=1)
         Recheck Cond: (jdoc @> '{"name": "N9WP5txmVu"}'::jsonb)
         ->  Bitmap Index Scan on idx_jtest3  (cost=0.00..81.75 rows=5051 width=0) (actual time=0.106..0.106 rows=1 loops=1)
               Index Cond: (jdoc @> '{"name": "N9WP5txmVu"}'::jsonb)
 Planning time: 0.144 ms
   (slice0)    Executor memory: 69K bytes.
   (slice1)    Executor memory: 305K bytes avg x 2 workers, 309K bytes max (seg1).  Work_mem: 9K bytes max.
 Memory used:  2047000kB
 Optimizer: Postgres query optimizer
 Execution time: 1.291 ms
(11 rows)

			
```

下面是 Python 访问的一个例子：

```
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

