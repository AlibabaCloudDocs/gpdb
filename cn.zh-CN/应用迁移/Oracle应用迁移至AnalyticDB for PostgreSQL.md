# Oracle应用迁移至AnalyticDB for PostgreSQL

AnalyticDB for PostgreSQL对Oracle 语法有着较好的兼容，本文介绍如何将Oracle应用迁移到AnalyticDB for PostgreSQL。

## 基于ora2pg完成初步转换工作

可以使用开源工具[ora2pg](https://github.com/darold/ora2pg)进行最初的Oracle应用转换。您可以使用ora2pg将Oracle的表DDL，view，package等语法转换成PostgreSQL兼容的语法。具体操作方法请参见ora2pg的用户文档。

**说明：** 由于脚本转换后的PG语法版本比AnalyticDB for PostgreSQL使用的PG内核版本高，而且ora2pg依赖规则进行转换，难免会存在偏差，因此您还需要手动对转换后的SQL脚本做纠正。

## 使用Orafunc插件

AnalyticDB for PostgreSQL中提供了Orafunc插件，该插件提供了一些兼容Oracle的函数。对于这些函数，您无需任何修改转换即可在AnalyticDB for PostgreSQL中使用。

在使用Orafunc插件前，只需执行`create extension orafunc;`命令即可。

```
postgres=> create extension orafunc;
CREATE EXTENSION
```

Orafunc插件提供的兼容函数如下表所示。

|函数|描述|示例|
|--|--|--|
|`nvl(anyelement, anyelement)`|-   若第一个参数为null，则会返回第二个参数。
-   若第一个参数不为null，则返回第一个参数。

 **说明：** 两个参数必须是相同类型。

|```
postgres=# select nvl(null,1);
nvl
-----
  1
(1 row)

postgres=# select nvl(0,1);
nvl
-----
  0
(1 row)

postgres=# select nvl(0,null);
nvl
-----
  0
(1 row)
``` |
|`add_months(day date, value int)RETURNS date`|在第一个月份参数上加上第二个月份参数，返回类型为date。|```
postgres=# select add_months(current_date, 2);
add_months
------------
2019-08-31
(1 row)
``` |
|`last_day(value date)`|返回某年某个月份的最后一天，返回类型为date。|```
postgres=# select last_day('2018-06-01');
 last_day
------------
2018-06-30
(1 row)
``` |
|`next_day(value date, weekday text)`|-   参数一：开始的日期。
-   参数二：包含星期几的英文字符串，如Friday。

 返回开始日期后的第二个星期几的日期，如第二个Friday。|```
postgres=# select next_day(current_date, 'FRIDAY');
 next_day
------------
2019-07-05
(1 row)
``` |
|`next_day(value date, weekday integer)`|-   参数一：开始的日期。
-   参数二：星期几的数字，取值为1到7，1为星期日，2为星期一，以此类推。

 返回开始日期加天数之后的日期。|```
postgres=# select next_day('2019-06-22', 1);
 next_day
------------
2019-06-23
(1 row)

postgres=# select next_day('2019-06-22', 2);
 next_day
------------
2019-06-24
(1 row)
``` |
|`months_between(date1 date, date2 date)`|返回date1和date2之间的月数。 -   如果date1晚于date2，结果为正。
-   如果date1早于date2，结果为负。

|```
postgres=# select months_between('2019-01-01', '2018-11-01');
months_between
----------------
             2
(1 row)

postgres=# select months_between('2018-11-01', '2019-01-01');
months_between
----------------
            -2
(1 row)
``` |
|`trunc(value timestamp with time zone, fmt text)`|-   参数一：要被截断的timestamp。
-   参数二：应用于截断的度量单位，如年，月，日，周，时，分，秒等。
    -   Y：截断成日期年份的第一天。
    -   Q：返回季度的第一天。

|```
postgres=# SELECT TRUNC(current_date,'Q');
  trunc
------------
2019-04-01
(1 row)

postgres=# SELECT TRUNC(current_date,'Y');
  trunc
------------
2019-01-01
(1 row)
``` |
|`trunc(value timestamp with time zone)`|截断timestamp，默认截断时分秒。|```
postgres=# SELECT TRUNC('2019-12-11'::timestamp);
        trunc
------------------------
2019-12-11 00:00:00+08
(1 row)
``` |
|`trunc(value date)`|截断日期。|```
postgres=# SELECT TRUNC('2019-12-11'::timestamp,'Y');
        trunc
------------------------
2019-01-01 00:00:00+08
``` |
|`round(value timestamp with time zone, fmt text)`|将timestamp圆整到最近的unit\_of\_measure\(日，周等\)。|```
postgres=# SELECT round('2018-10-06 13:11:11'::timestamp, 'YEAR');
        round
------------------------
2019-01-01 00:00:00+08
(1 row)
``` |
|`round(value timestamp with time zone)`|默认圆整到天。|```
postgres=# SELECT round('2018-10-06 13:11:11'::timestamp);
        round
------------------------
2018-10-07 00:00:00+08
(1 row)
``` |
|`round(value date, fmt text)`|参数类型为date。|```
postgres=# SELECT round(TO_DATE('27-OCT-00','DD-MON-YY'), 'YEAR');
  round
------------
2001-01-01
(1 row)

postgres=# SELECT round(TO_DATE('27-FEB-00','DD-MON-YY'), 'YEAR');
  round
------------
2000-01-01
(1 row)
``` |
|`round(value date)`|参数类型为date。|```
ostgres=# SELECT round(TO_DATE('27-FEB-00','DD-MON-YY'));
  round
------------
2000-02-27
(1 row)
``` |
|`instr(str text, patt text, start int, nth int)`|在一个string中搜索一个substring，若搜索到则返回substring在string中位置，若没有搜索到，则返回0。 -   start：搜索的起始位置。
-   nth：搜索第几次出现的位置。

|```
postgres=# SELECT instr('Greenplum', 'e',1,2);
instr
-------
    4
(1 row)

postgres=# SELECT instr('Greenplum', 'e',1,1);
instr
-------
    3
(1 row)
``` |
|`instr(str text, patt text, start int)`|未提供nth参数，默认是第一次出现的位置。|```
postgres=# SELECT instr('Greenplum', 'e',1);
instr
-------
    3
(1 row)
``` |
|`instr(str text, patt text)`|未提供start参数，默认从头开始搜索。|```
postgres=# SELECT instr('Greenplum', 'e');
instr
-------
    3
(1 row)
``` |
|`reverse(str text, start int, _end int)`|str为输入的字符串start；end分别为对字符串从start到end这一段进行逆序。|```
postgres=> select reverse('adb4pg', 5,6);
reverse
---------
gp
``` |
|`reverse(str text, start int)`|从start开始到字符串结束进行逆序。|```
ostgres=> select reverse('adb4pg', 4);
reverse
---------
gp4
``` |
|`reverse(str text)`|逆序整个字符串。|```
postgres=> select reverse('adb4pg');
reverse
---------
gp4bda
(1 行记录)
``` |
|`concat(text, text)`|将两个字符串拼接在一起。|```
postgres=> select concat('adb','4pg');
concat
--------
adb4pg
(1 行记录)
``` |
|`concat(text, anyarray)/concat(anyarray, text)/concat(anyarray, anyarray)`|用于拼接任意类型的数据。|```
postgres=> select concat('adb4pg', 6666);
  concat
------------
adb4pg6666
(1 行记录)

postgres=> select concat(6666, 6666);
 concat
----------
66666666
(1 行记录)

postgres=> select concat(current_date, 6666);
    concat
----------------
2019-06-306666
(1 行记录)
``` |
|`nanvl(float4, float4)/nanvl(float4, float4)/nanvl(numeric, numeric)`|如果第一个参数为数值类型，则返回第一个参数；如果不为数值类型，则返回第二参数。|```
postgres=> select nanvl('NaN', 1.1);
nanvl
-------
  1.1
(1 行记录)

postgres=> select nanvl('1.2', 1.1);
nanvl
-------
  1.2
(1 行记录)
``` |
|`bitand(bigint, bigint)`|将两个整形的二进制进行and操作，并返回and之后的结果，只输出一行。|```
postgres=# select bitand(1,3);
bitand
--------
     1
(1 row)

postgres=# select bitand(2,6);
bitand
--------
     2
(1 row)

postgres=# select bitand(4,6);
bitand
--------
     4
(1 row)
``` |
|`listagg1_transfn(text, text)`|最后输出一个数组（多行），第二个参数会放在每行结果的最后，相当于分割符。|```
postgres=> SELECT listagg1_transfn(t, '.') FROM (VALUES('abc'), ('def')) as l(t);
listagg1_transfn
------------------
abc.def.
``` |
|`listagg2_transfn(text, text, text)`|最后输出一个数组（多行），第二个参数会放在每行结果的最后，相当于分割符，第三个参数会和第一个参数做聚集。|-|
|`listagg(text)`|将文本值聚集成一个串。|```
postgres=> SELECT listagg(t) FROM (VALUES('abc'), ('def')) as l(t);
listagg
---------
abcdef
(1 行记录)
``` |
|`listagg(text, text)`|将文本值聚集成一个串，第二个参数执行了分割符。|```
postgres=> SELECT listagg(t, '.') FROM (VALUES('abc'), ('def')) as l(t);
listagg
---------
abc.def
(1 行记录)
``` |
|`nvl2(anyelement, anyelement, anyelement)`|如果第一个参数不为null，那么返回第二个参数，如果第一个参数为null，则返回第三个参数。|```
postgres=> select nvl2(null, 1, 2);
nvl2
------
   2
(1 行记录)

postgres=> select nvl2(0, 1, 2);
nvl2
------
   1
(1 行记录)
``` |
|`lnnvl(bool)`|如果参数为null或者false，则返回true；如果为true，则返回false。|```
postgres=> select lnnvl(null);
lnnvl
-------
t
(1 行记录)

postgres=> select lnnvl(false);
lnnvl
-------
t
(1 行记录)

postgres=> select lnnvl(true);
lnnvl
-------
f
(1 行记录)
``` |
|`dump("any")`|返回一个文本值，该文本中包含第一个参数的数据类型代码、以字节计的长度和内部表示。|```
postgres=> select dump('adb4pg');
                dump
---------------------------------------
Typ=705 Len=7: 97,100,98,52,112,103,0
(1 行记录)
``` |
|`dump("any", integer)`|第二个参数表示返回文本值的内部表示是使用10进制还是16进制，目前仅支持10进制和16进制。|```
postgres=> select dump('adb4pg', 10);
                dump
---------------------------------------
Typ=705 Len=7: 97,100,98,52,112,103,0
(1 行记录)

postgres=> select dump('adb4pg', 16);
               dump
------------------------------------
Typ=705 Len=7: 61,64,62,34,70,67,0
(1 行记录)

postgres=> select dump('adb4pg', 2);
ERROR:  unknown format (others.c:430)
``` |
|`nlssort(text, text)`|指定排序规则的排序数据函数。|```
create table t1 (name text);
INSERT INTO t1 VALUES('Anne'), ('anne'), ('Bob'), ('bob');
postgres=> select from t1 order by nlssort(name, 'en_US.UTF-8');
name
------
anne
Anne
bob
Bob
(4 行记录)

postgres=> select from t1 order by nlssort(name, 'C');
name
------
Anne
Bob
anne
bob
(4 行记录)
``` |
|`substr(str text, start int)`|获取参数一中字符串的子串。参数二表示获取到子串的起始位置 \>= start。|```
postgres=> select substr('adb4pg', 1);
substr
--------
adb4pg
(1 行记录)

postgres=> select substr('adb4pg', 4);
substr
--------
4pg
(1 行记录)
``` |
|`substr(str text, start int, len int)`|参数三会指定子串的结束位置 `>= start and <= end`。|```
postgres=> select substr('adb4pg', 5,6);
substr
--------
pg
(1 行记录)
``` |
|`pg_catalog.substrb(varchar2, integer, integer)`|varchar2类型的获取子串函数；参数二为start pos，参数三为end pos。|```
postgres=> select substr('adb4pg'::varchar2, 5,6) ;
substr
--------
pg
(1 行记录)
``` |
|`pg_catalog.substrb(varchar2, integer)`|varchar2类型的获取子串函数；参数二为start pos，从start pos一直取到字符串结束。|```
postgres=> select substr('adb4pg'::varchar2, 4) ;
substr
--------
4pg
(1 行记录)
``` |
|`pg_catalog.lengthb(varchar2)`|获取varchar2类型字符串占的字节数。若输入为null返回null，输入为空字符，则返回0。|```
ostgres=> select lengthb('adb4pg'::varchar2) ;
lengthb
---------
      6
(1 行记录)

postgres=> select lengthb('分析型'::varchar2) ;
lengthb
---------
      9
(1 行记录)
``` |

Orafunc插件除了提供上述兼容函数，还对Oracle的Varchar2数据类型提供了兼容。

对于以下的四个Oracle函数，在AnalyticDB for PostgreSQL中，无需安装Orafunc插件就可以提供兼容支持。

|函数|描述|示例|
|--|--|--|
|`sinh(float)`|双曲正弦值。|```
postgres=# select sinh(0.1);
      sinh
-------------------
0.100166750019844
(1 row)
``` |
|`tanh(float)`|双曲正切值。|```
postgres=# select tanh(3);
      tanh
------------------
0.99505475368673
(1 row)
``` |
|`cosh(float)`|双曲余弦值。|```
postgres=# select cosh(0.2);
      cosh
------------------
1.02006675561908
(1 row)
``` |
|`decode(expression, value, return [,value,return]... [, default])`|在表达式中寻找一个搜索值，搜索到则返回指定的值。如果没有搜索到，返回默认值。|```
create table t1(id int, name varchar(20));
postgres=# insert into t1 values(1,'alibaba');
postgres=# insert into t1 values(2,'adb4pg');
postgres=# select decode(id, 1, 'alibaba', 2, 'adb4pg', 'not found') from t1;
 case
---------
alibaba
adb4pg
(2 rows)

postgres=# select decode(id, 3, 'alibaba', 4, 'adb4pg', 'not found') from t1;
  case
-----------
not found
not found
(2 rows)
``` |

## 数据类型转换对照表

|Oracle|AnalyticDB for PostgreSQL|
|------|-------------------------|
|VARCHAR2|varchar or text|
|DATE|timestamp|
|LONG|text|
|LONG RAW|bytea|
|CLOB|text|
|NCLOB|text|
|BLOB|bytea|
|RAW|bytea|
|ROWID|oid|
|FLOAT|double precision|
|DEC|decimal|
|DECIMAL|decimal|
|DOUBLE PRECISION|double precision|
|INT|int|
|INTERGE|integer|
|REAL|real|
|SMALLINT|smallint|
|NUMBER|numeric|
|BINARY\_FLOAT|double precision|
|BINARY\_DOUBLE|double precision|
|TIMESTAMP|timestamp|
|XMLTYPE|xml|
|BINARY\_INTEGER|integer|
|PLS\_INTEGER|integer|
|TIMESTAMP WITH TIME ZONE|timestamp with time zone|
|TIMESTAMP WITH LOCAL TIME ZONE|timestamp with time zone|

## 系统函数转换对照表

|Oracle|AnalyticDB for PostgreSQL|
|------|-------------------------|
|sysdate|current timestamp|
|trunc|trunc/ date trunc|
|dbms\_output.put\_line|raise 语句|
|decode|转成case when/直接使用decode|
|NVL|coalesce|

## PL/SQL迁移指导

PL/SQL（Procedural Language/SQL）是一种过程化的SQL语言，是Oracle对SQL语句的拓展。PL/SQL使得SQL的使用可以具有一般编程语言的特点，可以用来实现复杂的业务逻辑。PL/SQL对应了AnalyticDB for PostgreSQL中的PL/PGSQL。

**Package**

PL/PGSQL不支持Package，需要把Package转换成schema，同时Package里面的所有procedure和function也需要转换成AnalyticDB for PostgreSQL的function。

例如：

```
create or replace package pkg is 

…

end;
```

转换成：

```
create schema pkg;
```

-   Package定义的变量

    procedure/function的局部变量保持不变，全局变量在AnalyticDB for PostgreSQL中可以使用临时表进行保存。

-   Package初始化块

    请删除，若无法删除请使用function封装，在需要的时候主动调用该function。

-   Package内定义的procedure/function

    Package内定义的procedure和function需要转成AnalyticDB for PostgreSQL的function，并把function定义到package对应的schema内。

    例如，有一个Package名为pkg中有如下函数：

    ```
    FUNCTION test_func (args int) RETURN int is
    var number := 10;
    BEGIN
    … …
    END;
    ```

    转换成如下AnalyticDB for PostgreSQL的function：

    ```
    CREATE OR REPLACE FUNCTION pkg. test_func(args int) RETURNS int AS
    $$
    
      … …
    
    $$
     LANGUAGE plpgsql;
    ```


**Procedure/function**

Oracle中的procedure和function，不论属于package的还是属于全局，都需要转换成AnalyticDB for PostgreSQL的function。

例如：

```
CREATE OR REPLACE FUNCTION test_func (v_name varchar2, v_version varchar2)
RETURN varchar2 IS
    ret varchar(32);
BEGIN
    IF v_version IS NULL THEN
        ret := v_name;
ELSE
    ret := v_name || '/' || v_version;
    END IF;
    RETURN ret;
END;
```

转化成：

```
CREATE OR REPLACE FUNCTION test_func (v_name varchar, v_version varchar)
RETURNS varchar AS
$$

DECLARE
    ret varchar(32);
BEGIN
    IF v_version IS NULL THEN
        ret := v_name;
ELSE
    ret := v_name || '/' || v_version;
    END IF;
    RETURN ret;
END;

$$
 LANGUAGE plpgsql;
```

Procedure/function转换的注意事项有以下几点：

-   RETURN关键字转成RETURNS。
-   函数体使用&dollar;\\$ ... &dollar;\\$进行封装。
-   函数语言声明。
-   Subprocedure需要转换成AnalyticDB for PostgreSQL的function。

**PL statement**

-   **For语句**

    PL/SQL和PL/PGSQL在带有REVERSE的整数FOR循环中的工作方式不同：

    -   PL/SQL中是从第二个数向第一个数倒数。
    -   PL/PGSQL是从第一个数向第二个数倒数。
    因此在移植时需要交换循环边界，如下所示：

    ```
    FOR i IN REVERSE 1..3 LOOP
        DBMS_OUTPUT.PUT_LINE (TO_CHAR(i));
    END LOOP;
    ```

    转换成：

    ```
    FOR i IN REVERSE 3..1 LOOP
        RAISE ‘%’ ,i;
    END LOOP;
    ```

-   **PRAGMA语句**

    AnalyticDB for PostgreSQL中没有PRAGMA语句，删除该类语句。

-   **事务处理**

    AnalyticDB for PostgreSQL的function 内部无法使用事务控制语句，如begin，commit，rollback等。

    修改方法如下：

    -   删除函数体内的事务控制语句，把事务控制放在函数体外。
    -   把函数按照commit/rollback 拆分成多个。
-   **EXECUTE语句**

    AnalyticDB for PostgreSQL支持类似Oracle的动态SQL语句，不同之处如下：

    -   不支持using语法，可通过把参数拼接到SQL串中来解决。
    -   数据库标识符使用quote\_ident包裹，数值使用quote\_literal包裹。
    示例：

    ```
    EXECUTE 'UPDATE employees_temp SET commission_pct = :x' USING a_null;
    ```

    转换成：

    ```
    EXECUTE 'UPDATE employees_temp SET commission_pct = ' || quote_literal(a_null);
    ```

-   **Pipe row**

    Pipe row函数，可使用AnalyticDB for PostgreSQL的table function函数来替换。

    示例：

    ```
    TYPE pair IS RECORD(a int, b int);
    TYPE numset_t IS TABLE OF pair;
    
    FUNCTION f1(x int) RETURN numset_t PIPELINED IS
    DECLARE
        v_p pair;
    BEGIN
        FOR i IN 1..x LOOP
          v_p.a := i;
          v_p.b := i+10;
          PIPE ROW(v_p);
        END LOOP;
        RETURN;
    END;
    
    select * from f1(10);
    ```

    转换成：

    ```
    create type pair as (a int, b int);
    
    create or replace function f1(x int) returns setof pair as
    $$
    
    declare
    rec pair;
    begin
        for i in 1..x loop
            rec := row(i, i+10);
            return next rec;
        end loop;
        return ;
    end
    
    $$
     language 'plpgsql';
    
    select * from f1(10);
    ```

    说明：

    -   自定义类型pair转换成AnalyticDB for PostgreSQL的复合类型pair。
    -   Table of类型不需要定义，使用AnalyticDB for PostgreSQL的setof替换。
    -   Pipe row 语句转换成下面两个语句：

        ```
          rec := row(i);
          return next rec;
        ```

        oracle function还可以转换成如下语句：

        ```
        create or replace function f1(x int) returns setof record as
        $$
        
        declare
        rec record;
        begin
            for i in 1..x loop
                rec := row(i, i+10);
                return next rec;
            end loop;
            return ;
        end
        
        $$
        language 'plpgsql';
        ```

        与第一种改法的不同的是，这种改法不需要提前定义数据类型numset\_t，所以在查询的时候需要指定返回的类型，例如：`select * from f1(10) as (a int, b int);`。

-   **异常处理**
    -   使用raise抛出异常。
    -   Catch异常后，不能rollback事务，只能在udf外做rollback。
    -   AnalyticDB for PostgreSQL支持的error，请参考：[https://www.postgresql.org/docs/8.3/errcodes-appendix.html](https://www.postgresql.org/docs/8.3/errcodes-appendix.html)
-   **function中同时有Return和OUT参数**

    在AnalyticDB for PostgreSQL中，fucntion无法同时存在return和out参数，需要把返回的参数改写成out类型参数。

    示例：

    ```
    CREATE OR REPLACE FUNCTION test_func(id int, name varchar(10), out_id out int) returns varchar(10)
    AS $body$
    BEGIN
          out_id := id + 1;
          return name;
    end
    $body$
    LANGUAGE PLPGSQL;
    ```

    转换成：

    ```
    CREATE OR REPLACE FUNCTION test_func(id int, name varchar(10), out_id out int, out_name out varchar(10))
    AS $body$
    BEGIN
          out_id := id + 1;
          out_name := name;
    end
    $body$
    LANGUAGE PLPGSQL;
    ```

    `select * from test_func(1,’1’) into rec;`从rec中取对应字段的返回值即可。

-   **字符串连接中变量含有单引号**

    在如下示例中，变量param2是一个字符串类型。假设param2的值为`adb'-'pg`。下面的sql\_str直接放到AnalyticDB for PostgreSQL中使用会将`-`识别成一个operator而报错。需要使用quote\_literal函数来进行转换。

    示例：

    ```
    sql_str := 'select * from test1 where col1 = ' || param1 || ' and col2 = '''|| param2 || '''and col3 = 3';
    ```

    转换成：

    ```
    sql_str := 'select * from test1 where col1 = ' || param1 || ' and col2 = '|| quote_literal(param2) || 'and col3 = 3';
    ```

-   **获取两个timestamp相减后的天数**

    示例：

    ```
    SELECT to_date('2019-06-30 16:16:16') – to_date('2019-06-29 15:15:15') + 1 INTO v_days from dual;
    ```

    转换成：

    ```
    SELECT extract('days' from '2019-06-30 16:16:16'::timestamp - '2019-06-29 15:15:15'::timestamp + '1 days'::interval)::int INTO v_days;
    ```


**PL数据类型**

-   **Record**

    使用AnalyticDB for PostgreSQL的复合数据类型替换。

    示例：

    ```
    TYPE rec IS RECORD (a int, b int);
    ```

    转换成：

    ```
    CREATE TYPE rec AS (a int, b int);
    ```

-   **Nest table**
    -   Nest table作为PL变量，可以使用ADB for PG的array类型替换。

        示例：

        ```
        DECLARE
          TYPE Roster IS TABLE OF VARCHAR2(15);
          names Roster :=
          Roster('D Caruso', 'J Hamil', 'D Piro', 'R Singh');
        BEGIN
          FOR i IN names.FIRST .. names.LAST
          LOOP
              IF names(i) = 'J Hamil' THEN
                DBMS_OUTPUT.PUT_LINE(names(i));
              END IF;
          END LOOP;
        END;
        ```

        转换成：

        ```
        create or replace function f1() returns void as
        $$
        
        declare
            names varchar(15)[] := '{"D Caruso", "J Hamil", "D Piro", "R Singh"}';
            len int := array_length(names, 1);
        begin
            for i in 1..len loop
                if names[i] = 'J Hamil' then
                    raise notice '%', names[i];
                end if;
            end loop;
            return ;
        end
        
        $$
         language 'plpgsql';
        
        select f();
        ```

    -   作为function返回值，可以使用table function替换。
-   **Associative Array**

    无替换类型。

-   **Variable-Size Arrays**

    与Nest table类似，使用array类型替换。

-   **Global variables**

    目前AnalyticDB for PostgreSQL不支持global variables，可以把package中的所有global variables存入一张临时表（temporary table）中，然后修改定义，获取global variables的函数。

    示例：

    ```
    create temporary table global_variables (
            id int,
            g_count int,
            g_set_id varchar(50),
            g_err_code varchar(100)
    );
    
    insert into global_variables values(0, 1, null，null);
    
    CREATE OR REPLACE FUNCTION get_variable() returns setof global_variables AS
    
    $$
    
    DECLARE
        rec global_variables%rowtype;
    BEGIN
        execute 'select * from global_variables' into rec;
        return next rec;
    END;
    
    $$
     LANGUAGE plpgsql;
    
    CREATE OR REPLACE FUNCTION set_variable(in param varchar(50), in value anyelement) returns void AS
    
    $$
    
    BEGIN
        execute 'update global_variables set ' ||  quote_ident(param) || ' = ' || quote_literal(value);
    END;
    
    $$
     LANGUAGE plpgsql;
    ```

    临时表global\_variables中，字段id为这个表的分布列，由于AnalyticDB for PostgreSQL中不允许对于分布列的修改，需要加一个`tmp_rec record;`字段。

    修改一个全局变量时，使用：`select * from set_variable(‘g_error_code’, ‘error’::varchar) into tmp_rec;`。

    获取一个全局变量时，使用：`select * from get_variable() into tmp_rec; error_code := tmp_rec.g_error_code;`。


**SQL**

-   **Connect by**

    Oracle层次查询，AnalyticDB for PostgreSQL没有等价替换的SQL语句。可以使用循环按层次遍历这样的转换思路。

    示例：

    ```
    create table employee(
           emp_id numeric(18),
           lead_id numeric(18),
           emp_name varchar(200),
           salary numeric(10,2),
           dept_no varchar(8)
    );
    insert into employee values('1',0,'king','1000000.00','001');
    insert into employee values('2',1,'jack','50500.00','002');
    insert into employee values('3',1,'arise','60000.00','003');
    insert into employee values('4',2,'scott','30000.00','002');
    insert into employee values('5',2,'tiger','25000.00','002');
    insert into employee values('6',3,'wudde','23000.00','003');
    insert into employee values('7',3,'joker','21000.00','003');
    insert into employee values('3',7,'joker','21000.00','003');
    ```

    ```
    select emp_id,lead_id,emp_name,prior emp_name as lead_name,salary
         from employee
         start with  lead_id=0
         connect by prior emp_id =  lead_id
    ```

    转换成：

    ```
    create or replace function f1(tablename text, lead_id int, nocycle boolean) returns setof employee as
    $$
    
    declare
        idx int := 0;
        res_tbl varchar(265) := 'result_table';
        prev_tbl varchar(265) := 'tmp_prev';
        curr_tbl varchar(256) := 'tmp_curr';
    
        current_result_sql varchar(4000);
        tbl_count int;
    
        rec record;
    begin
    
        execute 'truncate ' || prev_tbl;
        execute 'truncate ' || curr_tbl;
        execute 'truncate ' || res_tbl;
        loop
            -- 查询当前层次结果，并插入到tmp_curr表
            current_result_sql := 'insert into ' || curr_tbl || ' select t1.* from ' || tablename || ' t1';
    
            if idx > 0 then
                current_result_sql := current_result_sql || ', ' || prev_tbl || ' t2 where t1.lead_id = t2.emp_id';
            else
                current_result_sql := current_result_sql || ' where t1.lead_id = ' || lead_id;
            end if;
            execute current_result_sql;
    
            -- 如果有环，删除已经遍历过的数据
            if nocycle is false then
                execute 'delete from ' || curr_tbl || ' where (lead_id, emp_id) in (select lead_id, emp_id from ' || res_tbl || ') ';
            end if;
    
            -- 如果没有数据，则退出
            execute 'select count(*) from ' || curr_tbl into tbl_count;
            exit when tbl_count = 0;
    
            -- 把tmp_curr数据保存到result表
            execute 'insert into ' || res_tbl || ' select * from ' || curr_tbl;
            execute 'truncate ' || prev_tbl;
            execute 'insert into ' || prev_tbl || ' select * from ' || curr_tbl;
            execute 'truncate ' || curr_tbl;
            idx := idx + 1;
        end loop;
    
        -- 返回结果
        current_result_sql := 'select * from ' || res_tbl;
        for rec in execute current_result_sql loop
            return next rec;
        end loop;
        return;
    end
    
    $$
     language plpgsql;
    ```

-   **Rownum**
    1.  限定查询结果集大小，可以使用limit替换。

        示例：

        ```
        select * from t where rownum < 10;
        ```

        转换成：

        ```
        select * from t limit 10;
        ```

    2.  使用row\_number\(\) over\(\)生成rownum。

        示例：

        ```
        select rownum, * from t;
        ```

        转换成：

        ```
        select row_number() over() as rownum, * from t;
        ```

-   **Dual表**
    1.  去掉dual。

        示例：

        ```
        select sysdate from dual;
        ```

        转换成：

        ```
        select current_timestamp;
        ```

    2.  创建一个叫dual的表。
-   **Select中的udf**

    AnalyticDB for PostgreSQL支持在select中调用udf，但是udf中不能有SQL语句，否则会收到如下的错误信息：

    ```
    ERROR: function cannot execute on segment because it accesses relation "public.t2" (functions.c:155) (seg1 slice1 127.0.0.1:25433 pid=52153) (cdbdisp.c:1326)
    DETAIL:
    SQL statement "select b from t2 where a = $1 "
    ```

    可以把select中的udf转换成SQL表达式或者子查询等方法来转换。

    示例：

    ```
    create or replace FUNCTION f1(arg int) RETURN int IS
        v int;
    BEGIN
        select b into v from t2 where a = arg;
        return v;
    END;
    
    select a, f1(b) from t1;
    ```

    转换成：

    ```
    select t1.a, t2.b from t1, t2 where t1.b = t2.a;
    ```

-   **（+）多表外链接**

    AnalyticDB for PostgreSQL不支持\(+\)语法形式，需要转换成标准的outer join语法。

    示例：

    ```
    oracle
    select * from a,b where a.id=b.id(+)
    ```

    转换成：

    ```
    select * from a left join b on a.id=b.id
    ```

    若在\(+\)中有三表的join，需要先用wte做两表的join，再用+号那个表跟wte表做outer join。

    示例：

    ```
    Select * from test1 t1, test2 t2, test3 t3 where t1.col1(+) between NVL(t2.col1, t3.col1) and NVL(t3.col1, t2.col1);
    ```

    转换成：

    ```
    with cte as (select t2.col1 as low, t2.col2, t3.col1 as high, t3.col2 as c2 from t2, t3)
    select * from t1 right outer join cte on t1.col1 between coalesce(cte.low, cte.high) and coalesce(cte.high,cte.low);
    ```

-   **Merge into**

    对于merge into语法的转换，要先在AnalyticDB for PostgreSQL中使用update进行更新，然后使用`GET DIAGNOSTICS rowcount := ROW_COUNT;`语句获取update更新的行数，若update更新的行数为0，再使用insert语句进行插入。

    ```
    MERGE INTO test1 t1
                USING (SELECT t2.col1 col1, t3.col2 col2,
                         FROM test2 t2, test3 t3) S
                ON S.col1 = 1 and S.col2 = 2
    WHEN MATCHED THEN
                  UPDATE
                  SET test1.col1 = S.col1+1,
                         test1.col2 = S.col2+2
    WHEN NOT MATCHED THEN
                  INSERT (col1, col2)
                  VALUES
                    (S.col1+1, S.col2+2);
    ```

    转换成：

    ```
    Update test1 t1 SET t1.col1 = test2.col1+1, test3.col2 = S.col2+2 where test2.col1 = 1 and test2.col2 = 2;
    GET DIAGNOSTICS rowcount := ROW_COUNT;
    if rowcount = 0 then
        insert into test1 values(test2.col1+1, test3.col2+2);
    end if;
    ```

-   **Sequence**

    示例：

    ```
    create sequence seq1;
    select seq1.nextval from dual;
    ```

    转换成：

    ```
    create SEQUENCE seq1;
    select nextval('seq1');
    ```

-   **Cursor的使用**
    -   在Oracle中，可以使用下面的语句对cursor进行遍历。

        示例：

        ```
        FUNCTION test_func() IS
            Cursor data_cursor IS SELECT * from test1;
        BEGIN
            FOR I IN data_cursor LOOP
                Do something with I;
        END LOOP;
        END;
        ```

        转换成：

        ```
        CREATE OR REPLACE FUNCTION test_func()
        AS $body$
        DECLARE
        data_cursor cursor for select * from test1;
        I record;
        BEGIN
            Open data_cursor;
            LOOP
               Fetch data_cursor INTO I;
              If not found then
                    Exit;
              End if;
              Do something with I;
            END LOOP;
            Close data_cursor;
        END;
        $body$
        LANGUAGE PLPGSQL;
        ```

    -   Oracle可以在递归调用的函数里重复打开名字相同的cursor。但是在AnalyticDB for PostgreSQL中则无法重发重复打开，需要改写成for I in query的形式。

        示例：

        ```
        FUNCTION test_func(level IN numer) IS
            Cursor data_cursor IS SELECT * from test1;
        BEGIN
        If level > 5 then
                return;
           End if;
        
            FOR I IN data_cursor LOOP
                Do something with I;
                test_func(level + 1);
        END LOOP;
        END;
        ```

        转换成：

        ```
        CREATE OR REPLACE FUNCTION test_func(level int) returns void
        AS $body$
        DECLARE
        data_cursor cursor for select * from test1;
        I record;
        BEGIN
            If level > 5 then
                return;
            End if;
            For I in select * from test1 LOOP
              Do something with I;
               PERFORM test_func(level+1);
            END LOOP;
        END;
        $body$
        LANGUAGE PLPGSQL;
        ```


