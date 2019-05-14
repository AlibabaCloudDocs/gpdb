# JSON 数据类型操作 {#concept_osb_pxy_52b .concept}

JSON 类型几乎已成为互联网及物联网（IoT）的基础数据类型，其重要性不言而喻，具体协议请参见 [JSON 官网](http://www.json.org/)。

PostgreSQL 对 JSON 的支持已经比较完善，阿里云深度优化云数据库 AnalyticDB for PostgreSQL，基于 PostgreSQL 语法进行了 JSON 数据类型的支持。

## 检查现有版本是否支持 JSON {#section_eyr_pxy_52b .section}

执行如下命令，检查是否已经支持 JSON：

```
=> SELECT '""'::json;
```

若系统出现如下信息，则说明已经支持 JSON 类型，可以使用实例了。若执行不成功，请重新启动实例。

```
 json 
------
 ""
(1 row)
```

若系统出现如下信息，则说明尚未支持 JSON 类型。

```
ERROR:  type "json" does not exist
LINE 1: SELECT '""'::json;
                     ^
```

上述命令是一次从字符串到 JSON 格式的强制转换，这也基本上是其操作上的本质。

## JSON 在数据库中的转换 {#section_iyr_pxy_52b .section}

数据库的操作主要分为读和写，JSON 的数据写入一般是字符串到 JSON。字符串中的内容必须符合 JSON 标准，包括字符串、数字、数组、对象等内容。如：

**字符串**

```
=> SELECT '"hijson"'::json;
 json  
-------
 "hijson"
(1 row)
```

`::`在 PostgreSQL/Greenplum/AnalyticDB for PostgreSQL 中代表强制类型转换。在此转换的时候，会调用 JSON 类型的输入函数。因此，类型转换时同样会做 JSON 格式的检查，如下所示：

```
=> SELECT '{hijson:1024}'::json;
ERROR:  invalid input syntax for type json
LINE 1: SELECT '{hijson:1024}'::json;
               ^
DETAIL:  Token "hijson" is invalid.
CONTEXT:  JSON data, line 1: {hijson...
=>
```

上述`"hijson"`两边的`"`是必不可少的，因为在标准中，KEY 值对应的是一个字符串，所以这里的`{hijson:1024}`在语法上会报错。

除了类型上的强制转换，还有从数据库记录到 JSON 串的转换。

我们正常使用 JSON，不会只用一个 String 或一个 Number，而是一个包含一个或多个键值对的对象。所以，对 Greenplum 而言，支持到对象的转换，即支持了 JSON 的绝大多数场景，如：

```
=> select row_to_json(row('{"a":"a"}', 'b'));
           row_to_json           
---------------------------------
 {"f1":"{\"a\":\"a\"}","f2":"b"}
(1 row)
=> select row_to_json(row('{"a":"a"}'::json, 'b'));
        row_to_json        
---------------------------
 {"f1":{"a":"a"},"f2":"b"}
(1 row)
```

由此也可以看出字符串和 JSON 的区别，这样就可以很方便地将一整条记录转换成 JSON。

## JSON 内部数据类型的定义 {#section_nws_dyy_52b .section}

-   对象

    对象是 JSON 中最常用的，如：

    ```
      => select '{"key":"value"}'::json;
            json       
      -----------------
       {"key":"value"}
      (1 row)
    ```

-   整数 & 浮点数

    JSON 的协议只有三种数字：整数、浮点数和常数表达式，当前 Greenplum 对这三种都有很好的支持。

    ```
      => SELECT '1024'::json;
       json 
      ------
       1024
      (1 row)
      => SELECT '0.1'::json;
       json 
      ------
       0.1
      (1 row)
    ```

    特殊情况下，需要如下信息：

    ```
      => SELECT '1e100'::json;
       json  
      -------
       1e100
      (1 row)
      => SELECT '{"f":1e100}'::json;
          json     
      -------------
       {"f":1e100}
      (1 row)
    ```

    并且，包括下面这个长度超长的数字：

    ```
      => SELECT '9223372036854775808'::json;
              json         
      ---------------------
       9223372036854775808
      (1 row)
    ```

-   数组

    ```
      => SELECT '[[1,2], [3,4,5]]'::json;
             json       
      ------------------
       [[1,2], [3,4,5]]
      (1 row)
    ```


## 操作符 {#section_bmb_fyy_52b .section}

**JSON 支持的操作符类型**

```
=> select oprname,oprcode from pg_operator where oprleft = 3114;
 oprname |          oprcode
---------+---------------------------
 ->      | json_object_field
 ->>     | json_object_field_text
 ->      | json_array_element
 ->>     | json_array_element_text
 #>      | json_extract_path_op
 #>>     | json_extract_path_text_op
(6 rows)
```

**基本使用方法**

```
=> SELECT '{"f":"1e100"}'::json -> 'f';
 ?column?
----------
 "1e100"
(1 row)
=> SELECT '{"f":"1e100"}'::json ->> 'f';
 ?column?
----------
 1e100
(1 row)
=> select '{"f2":{"f3":1},"f4":{"f5":99,"f6":"stringy"}}'::json#>array['f4','f6'];
 ?column?
-----------
 "stringy"
(1 row)
=> select '{"f2":{"f3":1},"f4":{"f5":99,"f6":"stringy"}}'::json#>'{f4,f6}';
 ?column?
-----------
 "stringy"
(1 row)
=> select '{"f2":["f3",1],"f4":{"f5":99,"f6":"stringy"}}'::json#>>'{f2,0}';
 ?column?
----------
 f3
(1 row)
```

## JSON 函数 {#section_exb_gyy_52b .section}

**支持的函数**

```
postgres=# \df *json*
                                                       List of functions
   Schema   |           Name            | Result data type |                    Argument data types                    |  Type
------------+---------------------------+------------------+-----------------------------------------------------------+--------
 pg_catalog | array_to_json             | json             | anyarray                                                  | normal
 pg_catalog | array_to_json             | json             | anyarray, boolean                                         | normal
 pg_catalog | json_array_element        | json             | from_json json, element_index integer                     | normal
 pg_catalog | json_array_element_text   | text             | from_json json, element_index integer                     | normal
 pg_catalog | json_array_elements       | SETOF json       | from_json json, OUT value json                            | normal
 pg_catalog | json_array_length         | integer          | json                                                      | normal
 pg_catalog | json_each                 | SETOF record     | from_json json, OUT key text, OUT value json              | normal
 pg_catalog | json_each_text            | SETOF record     | from_json json, OUT key text, OUT value text              | normal
 pg_catalog | json_extract_path         | json             | from_json json, VARIADIC path_elems text[]                | normal
 pg_catalog | json_extract_path_op      | json             | from_json json, path_elems text[]                         | normal
 pg_catalog | json_extract_path_text    | text             | from_json json, VARIADIC path_elems text[]                | normal
 pg_catalog | json_extract_path_text_op | text             | from_json json, path_elems text[]                         | normal
 pg_catalog | json_in                   | json             | cstring                                                   | normal
 pg_catalog | json_object_field         | json             | from_json json, field_name text                           | normal
 pg_catalog | json_object_field_text    | text             | from_json json, field_name text                           | normal
 pg_catalog | json_object_keys          | SETOF text       | json                                                      | normal
 pg_catalog | json_out                  | cstring          | json                                                      | normal
 pg_catalog | json_populate_record      | anyelement       | base anyelement, from_json json, use_json_as_text boolean | normal
 pg_catalog | json_populate_recordset   | SETOF anyelement | base anyelement, from_json json, use_json_as_text boolean | normal
 pg_catalog | json_recv                 | json             | internal                                                  | normal
 pg_catalog | json_send                 | bytea            | json                                                      | normal
 pg_catalog | row_to_json               | json             | record                                                    | normal
 pg_catalog | row_to_json               | json             | record, boolean                                           | normal
 pg_catalog | to_json                   | json             | anyelement                                                | normal
(24 rows)
```

**基本使用方法**

```
=> SELECT array_to_json('{{1,5},{99,100}}'::int[]);
  array_to_json
------------------
 [[1,5],[99,100]]
(1 row)
=> SELECT row_to_json(row(1,'foo'));
     row_to_json
---------------------
 {"f1":1,"f2":"foo"}
(1 row)
=> SELECT json_array_length('[1,2,3,{"f1":1,"f2":[5,6]},4]');
 json_array_length
-------------------
                 5
(1 row)
=> select * from json_each('{"f1":[1,2,3],"f2":{"f3":1},"f4":null,"f5":99,"f6":"stringy"}') q;
 key |   value
-----+-----------
 f1  | [1,2,3]
 f2  | {"f3":1}
 f4  | null
 f5  | 99
 f6  | "stringy"
(5 rows)
=> select json_each_text('{"f1":[1,2,3],"f2":{"f3":1},"f4":null,"f5":"null"}');
  json_each_text
-------------------
 (f1,"[1,2,3]")
 (f2,"{""f3"":1}")
 (f4,)
 (f5,null)
(4 rows)
=> select json_array_elements('[1,true,[1,[2,3]],null,{"f1":1,"f2":[7,8,9]},false]');
  json_array_elements
-----------------------
 1
 true
 [1,[2,3]]
 null
 {"f1":1,"f2":[7,8,9]}
 false
(6 rows)
create type jpop as (a text, b int, c timestamp);
=> select * from json_populate_record(null::jpop,'{"a":"blurfl","x":43.2}', false) q;
   a    | b | c
--------+---+---
 blurfl |   |
(1 row)
=>     select * from json_populate_recordset(null::jpop,'[{"a":"blurfl","x":43.2},{"b":3,"c":"2012-01-20 10:42:53"}]',false) q;
   a    | b |            c
--------+---+--------------------------
 blurfl |   |
        | 3 | Fri Jan 20 10:42:53 2012
(2 rows)
```

## 完整操作事例 {#section_o1r_gyy_52b .section}

**创建表**

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

**多表 JOIN**

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

## JSON 函数索引 {#section_vbs_pxy_52b .section}

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

**注意：**JSON 类型暂时不能支持作为分布键来使用；也不支持 JSON 聚合函数。

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

