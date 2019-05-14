# Operations of JSON data {#concept_osb_pxy_52b .concept}

The JSON type has become the standard data type of the Internet and the Internet of Things \(IoT\). You can view the specific protocols at the [JSON Official Website](http://www.json.org/). PostgreSQL supports JSON well. AnalyticDB for PostgreSQL also supports the JSON data type based on the PostgreSQL syntax.

This document introduces the basic operations and supported objects of the JSON data in AnalyticDB for PostgreSQL, including [checking compatibility](#check), [converting strings to JSON](#), [internal data types](#datatype), [operators](#operator), and [functions](#function). In addition, some [usage examples](#code) are provided for your reference.

## Check whether the current version supports JSON {#check .section}

Start a AnalyticDB for PostgreSQL instance, and run the following command to check whether the current version supports JSON or not:

```
=> SELECT '""'::json;
```

If the operation fails, restart the instance and run the preceding command again.

This command dictates a force type conversion from a string to the JSON format, and the following results indicates whether the JSON type is supported.

-   If the system prompts the following response, it indicates that the JSON type is supported and the instance is ready for use.

    ```
       json 
      ------
       ""
      (1 row)
    ```

-   If the system prompts the following response, it indicates that the JSON type is not supported yet.

    ```
      ERROR:  type "json" does not exist
      LINE 1: SELECT '""'::json;
                           ^
    ```


## JSON conversion in the database {#convert .section}

Database operations mainly involve: read and write. Writing JSON data means converting strings to JSON format. The content in the strings must conform to the JSON standard, including strings, numbers, arrays, and objects. For example:

**String**

```
=> SELECT '"hijson"'::json;
 json  
-------
 "hijson"
(1 row)
```

`::` represents force type conversion in PostgreSQL, Greenplum and AnalyticDB for PostgreSQL. The JSON type input function is called during the conversion process. Therefore, JSON format check is performed during the type conversion as follows:

```
=> SELECT '{hijson:1024}'::json;
ERROR:  invalid input syntax for type json
LINE 1: SELECT '{hijson:1024}'::json;
               ^
DETAIL:  Token "hijson" is invalid.
CONTEXT:  JSON data, line 1: {hijson...
=>
```

The aforementioned `"` are necessary for `"hijson"`. Because the JSON standard requires the KEY value to be a string, the `{hijson:1024}` here returns a syntax error.

Apart from the type conversion, the conversion from the database record to the JSON string is also performed.

We do not normally use only one string or one number for JSON, but an object that contains one or more key-value pairs. So for Greenplum, conversion to objects is applicable to a majority of JSON scenarios, such as:

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

We can also see the differences between the string and JSON here, so as to conveniently convert a full record into the JSON type.

## JSON internal data types {#datatype .section}

-   Object

    The object is the most frequently used data in JSON, such as:

    ```
      => select '{"key":"value"}'::json;
            json       
      -----------------
       {"key":"value"}
      (1 row)
    ```

-   Integer and floating point

    The JSON protocol only has three types of numbers: integer, floating point and constant expression. Greenplum provides good support for all three number types.

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

    The following information is required in some special situations:

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

    And the extra-long number is also included as follows:

    ```
     => SELECT '9223372036854775808'::json;
              json         
      ---------------------
       9223372036854775808
      (1 row)
    ```

-   Array

    ```
      => SELECT '[[1,2], [3,4,5]]'::json;
             json       
      ------------------
       [[1,2], [3,4,5]]
      (1 row)
    ```


## Operators {#operator .section}

**Operator types supported by JSON**

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

**Basic usage**

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

## JSON functions {#function .section}

**Supported functions**

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

**Basic usage**

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

## Code Examples {#code .section}

**Create a table**

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

**Multi-table JOIN**

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

**JSON function indexing**

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

**Note:** JSON type cannot be used as the distribution key for now and the JSON aggregate functions are not supported.

The following is an example of Python access:

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

