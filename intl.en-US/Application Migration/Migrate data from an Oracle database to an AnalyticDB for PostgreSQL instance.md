# Migrate data from an Oracle database to an AnalyticDB for PostgreSQL instance

This topic describes how to migrate data from an Oracle database to an AnalyticDB for PostgreSQL instance, which is compatible with the syntax of Oracle SQL statements.

## Syntax conversion by using Ora2Pg

[Ora2Pg](https://github.com/darold/ora2pg) is an open source tool. You can use it to convert DDL, view, and package statements in Oracle to PostgreSQL-compatible syntax. For more information, see the Ora2Pg documentation.

**Note:** However, you must manually correct converted SQL scripts because the PostgreSQL syntax version after script conversion is later than the kernel version of your AnalyticDB for PostgreSQL instance and the rules on which Ora2Pg depends for conversion may be missing or incorrect.

## orafce

AnalyticDB for PostgreSQL provides the orafce plug-in, which offers functions that are compatible with the syntax of Oracle SQL statements. These functions can be used by AnalyticDB for PostgreSQL without modifications or conversions.

Before using this plug-in, execute the `create extension orafce;` statement.

```
postgres=> create extension orafce;
CREATE EXTENSION
```

The following table lists the orafce-provided functions that are compatible with the syntax of Oracle SQL statements.

|Function|Description|Example|
|--------|-----------|-------|
|`nvl(anyelement, anyelement)`|-   If the value of the first parameter is null, this function returns the value of the second parameter.
-   If the value of the first parameter is not null, this function returns the value of the first parameter.

**Note:** The two parameters must be of the same data type.

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
|`add_months(day date, value int)RETURNS date`|This function adds the number of months \(specified by using the second parameter\) to a date \(specified by using the first parameter\) and returns a date.|```
postgres=# select add_months(current_date, 2);
add_months
------------
2019-08-31
(1 row)
``` |
|`last_day(value date)`|This function returns the last day of the month for a specified date. The return value is a date.|```
postgres=# select last_day('2018-06-01');
 last_day
------------
2018-06-30
(1 row)
``` |
|`next_day(value date, weekday text)`|-   The first parameter specifies a start date.
-   The second parameter specifies the day of a week, such as Friday.

This function returns the date that represents a day of the second week since the start date, such as the date that represents the second Friday.|```
postgres=# select next_day(current_date, 'FRIDAY');
 next_day
------------
2019-07-05
(1 row)
``` |
|`next_day(value date, weekday integer)`|-   The first parameter specifies a start date.
-   The second parameter specifies a number that represents a day of a week. The number ranges from 1 to 7. The value 1 represents Sunday, and the value 2 represents Monday. Similarly, the value 7 represents Saturday.

This function returns a date that is a specific number of days later than the start date.|```
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
|`months_between(date1 date, date2 date)`|This function returns the number of months between date1 and date2. -   If date1 is later than date2, the return value is positive.
-   If date1 is earlier than date2, the return value is negative.

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
|`trunc(value timestamp with time zone, fmt text)`|-   The first parameter specifies the timestamp to truncate.
-   The second parameter specifies the precision to which the timestamp is truncated, such as year, month, day, week, hour, minute, and second.
    -   Y: indicates that the timestamp is truncated to the first day of the year that corresponds to the timestamp.
    -   Q: indicates that the timestamp is truncated to the first day of the quarter that corresponds to the timestamp.

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
|`trunc(value timestamp with time zone)`|This function returns a timestamp that is truncated to a second by default.|```
postgres=# SELECT TRUNC('2019-12-11'::timestamp);
        trunc
------------------------
2019-12-11 00:00:00+08
(1 row)
``` |
|`trunc(value date)`|This function returns a timestamp that is truncated to a date.|```
postgres=# SELECT TRUNC('2019-12-11'::timestamp,'Y');
        trunc
------------------------
2019-01-01 00:00:00+08
``` |
|`round(value timestamp with time zone, fmt text)`|This function returns a timestamp that is rounded to the nearest unit such as week or day.|```
postgres=# SELECT round('2018-10-06 13:11:11'::timestamp, 'YEAR');
        round
------------------------
2019-01-01 00:00:00+08
(1 row)
``` |
|`round(value timestamp with time zone)`|This function returns a timestamp that is rounded to a day by default.|```
postgres=# SELECT round('2018-10-06 13:11:11'::timestamp);
        round
------------------------
2018-10-07 00:00:00+08
(1 row)
``` |
|`round(value date, fmt text)`|This function returns a rounded date.|```
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
|`round(value date)`|This function returns a rounded date.|```
ostgres=# SELECT round(TO_DATE('27-FEB-00','DD-MON-YY'));
  round
------------
2000-02-27
(1 row)
``` |
|`instr(str text, patt text, start int, nth int)`|This function searches for a substring within a string. If a substring is obtained, the function returns the position of the substring. Otherwise, it returns 0. -   start: specifies the start position of the search.
-   nth: specifies the position of the substring that appears for the nth time.

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
|`instr(str text, patt text, start int)`|If the nth parameter is not specified, this function returns the position of the substring that appears for the first time.|```
postgres=# SELECT instr('Greenplum', 'e',1);
instr
-------
    3
(1 row)
``` |
|`instr(str text, patt text)`|If the start parameter is not specified, this function starts to search for a substring at the beginning of a string.|```
postgres=# SELECT instr('Greenplum', 'e');
instr
-------
    3
(1 row)
``` |
|`plvstr.rvrs(str text, start int, _end int)`|This function reverses characters within a specified string. The str parameter specifies the string, and the start and end parameters specify the characters to reverse.|```
postgres=> select plvstr.rvrs('adb4pg', 5,6);
reverse
---------
gp
``` |
|`plvstr.rvrs(str text, start int)`|This function reverses characters from the character indicated by the start parameter to the end of the string.|```
ostgres=> select plvstr.rvrs('adb4pg', 4);
reverse
---------
gp4
``` |
|`plvstr.rvrs(str text)`|This function reverses an entire string.|```
postgres=> select plvstr.rvrs('adb4pg');
reverse
---------
gp4bda
(1 row)
``` |
|`concat(text, text)`|This function joins two strings together.|```
postgres=> select concat('adb','4pg');
concat
--------
adb4pg
(1 row)
``` |
|`concat(text, anyarray)/concat(anyarray, text)/concat(anyarray, anyarray)`|This function joins data of the same or different data types together.|```
postgres=> select concat('adb4pg', 6666);
  concat
------------
adb4pg6666
(1 row)

postgres=> select concat(6666, 6666);
 concat
----------
66666666
(1 row)

postgres=> select concat(current_date, 6666);
    concat
----------------
2019-06-306666
(1 row)
``` |
|`nanvl(float4, float4)/nanvl(float4, float4)/nanvl(numeric, numeric)`|If the first parameter is of the NUMERIC data type, this function returns the value of the first parameter. Otherwise, this function returns the value of the second parameter.|```
postgres=> select nanvl('NaN', 1.1);
nanvl
-------
  1.1
(1 row)

postgres=> select nanvl('1.2', 1.1);
nanvl
-------
  1.2
(1 row)
``` |
|`bitand(bigint, bigint)`|This function conducts an AND operation for two binary numbers of the INTEGER data type. Only one row is returned.|```
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
|`listagg(text)`|This function returns a clustered string for texts.|```
postgres=> SELECT listagg(t) FROM (VALUES('abc'), ('def')) as l(t);
listagg
---------
abcdef
(1 row)
``` |
|`listagg(text, text)`|This function returns a clustered string for texts. The value of the second parameter acts as a separator.|```
postgres=> SELECT listagg(t, '.') FROM (VALUES('abc'), ('def')) as l(t);
listagg
---------
abc.def
(1 row)
``` |
|`nvl2(anyelement, anyelement, anyelement)`|If the value of the first parameter is null, this function returns the value of the third parameter. Otherwise, this function returns the value of the second parameter.|```
postgres=> select nvl2(null, 1, 2);
nvl2
------
   2
(1 row)

postgres=> select nvl2(0, 1, 2);
nvl2
------
   1
(1 row)
``` |
|`lnnvl(bool)`|If the value of the parameter is null or false, this function returns true. If the value of the parameter is true, this function returns false.|```
postgres=> select lnnvl(null);
lnnvl
-------
t
(1 row)

postgres=> select lnnvl(false);
lnnvl
-------
t
(1 row)

postgres=> select lnnvl(true);
lnnvl
-------
f
(1 row)
``` |
|`dump("any")`|This function returns a text that contains the data type code, length in bytes, and internal representation of the first parameter.|```
postgres=> select dump('adb4pg');
                dump
---------------------------------------
Typ=705 Len=7: 97,100,98,52,112,103,0
(1 row)
``` |
|`dump("any", integer)`|The second parameter specifies the format of the return value. The format can be a decimal notation \(indicated by 10\) or a hexadecimal notation \(indicated by 16\).|```
postgres=> select dump('adb4pg', 10);
                dump
---------------------------------------
Typ=705 Len=7: 97,100,98,52,112,103,0
(1 row)

postgres=> select dump('adb4pg', 16);
               dump
------------------------------------
Typ=705 Len=7: 61,64,62,34,70,67,0
(1 row)

postgres=> select dump('adb4pg', 2);
ERROR:  unknown format (others.c:430)
``` |
|`nlssort(text, text)`|This function sorts data in a specific order.|```
create table t1 (name text);
INSERT INTO t1 VALUES('Anne'), ('anne'), ('Bob'), ('bob');
postgres=> select from t1 order by nlssort(name, 'en_US.UTF-8');
name
------
anne
Anne
bob
Bob
(4 rows)

postgres=> select from t1 order by nlssort(name, 'C');
name
------
Anne
Bob
anne
bob
(4 rows)
``` |
|`substr(str text, start int)`|This function retrieves a substring within the string specified by the first parameter. The second parameter specifies the start position of the substring.|```
postgres=> select substr('adb4pg', 1);
substr
--------
adb4pg
(1 row)

postgres=> select substr('adb4pg', 4);
substr
--------
4pg
(1 row)
``` |
|`substr(str text, start int, len int)`|The third parameter specifies the end position of the substring. The value of this parameter is greater than or equal to the value of the start parameter but is less than or equal to the value of start + len - 1.|```
postgres=> select substr('adb4pg', 5,6);
substr
--------
pg
(1 row)
``` |
|`pg_catalog.substrb(varchar2, integer, integer)`|This function returns a substring within a string of the VARCHAR2 data type. The second parameter specifies the start position of the substring, and the third parameter specifies the end position of the substring.|```
postgres=> select substr('adb4pg'::varchar2, 5,6) ;
substr
--------
pg
(1 row)
``` |
|`pg_catalog.substrb(varchar2, integer)`|This function returns a substring within a string of the VARCHAR2 data type. The substring starts from the character indicated by the second parameter and continues until the end of the string.|```
postgres=> select substr('adb4pg'::varchar2, 4) ;
substr
--------
4pg
(1 row)
``` |
|`pg_catalog.lengthb(varchar2)`|This function returns the number of bytes for a specific string that is of the VARCHAR2 data type. If null is specified, it returns null. If an empty string is specified, it returns 0.|```
ostgres=> select lengthb('adb4pg'::varchar2) ;
lengthb
---------
      6
(1 row)
``` |

In addition to the preceding functions, orafce is compatible with the VARCHAR2 data type in Oracle.

The four Oracle functions listed in the following table are compatible with AnalyticDB for PostgreSQL. Therefore, they can be used in AnalyticDB for PostgreSQL without installing the orafce plug-in.

|Function|Description|Example|
|--------|-----------|-------|
|`sinh(float)`|This function returns a hyperbolic sine value.|```
postgres=# select sinh(0.1);
      sinh
-------------------
0.100166750019844
(1 row)
``` |
|`tanh(float)`|This function returns a hyperbolic tangent value.|```
postgres=# select tanh(3);
      tanh
------------------
0.99505475368673
(1 row)
``` |
|`cosh(float)`|This function returns a hyperbolic cosine value.|```
postgres=# select cosh(0.2);
      cosh
------------------
1.02006675561908
(1 row)
``` |
|`decode(expression, value, return [,value,return]... [, default])`|This function searches for a value in expressions. If the value is found, the function returns it. Otherwise, the function returns the default value.|```
create table t1(id int, name varchar(20));
postgres=# insert into t1 values(1,'alibaba');
postgres=# insert into t1 values(2,'adb4pg');
postgres=# select decode(id, 1, 'alibaba', 2, 'adb4pg', 'not found') from t1;
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

## Mapping between Oracle data types and AnalyticDB for PostgreSQL data types

|Oracle|AnalyticDB for PostgreSQL|
|------|-------------------------|
|VARCHAR2|VARCHAR or TEXT|
|DATE|TIMESTAMP|
|LONG|TEXT|
|LONG RAW|BYTEA|
|CLOB|TEXT|
|NCLOB|TEXT|
|BLOB|BYTEA|
|RAW|BYTEA|
|ROWID|OID|
|FLOAT|DOUBLE PRECISION|
|DEC|DECIMAL|
|DECIMAL|DECIMAL|
|DOUBLE PRECISION|DOUBLE PRECISION|
|INT|INT|
|INTERGE|INTEGER|
|REAL|REAL|
|SMALLINT|SMALLINT|
|NUMBER|NUMERIC|
|BINARY\_FLOAT|DOUBLE PRECISION|
|BINARY\_DOUBLE|DOUBLE PRECISION|
|TIMESTAMP|TIMESTAMP|
|XMLTYPE|XML|
|BINARY\_INTEGER|INTEGER|
|PLS\_INTEGER|INTEGER|
|TIMESTAMP WITH TIME ZONE|TIMESTAMP WITH TIME ZONE|
|TIMESTAMP WITH LOCAL TIME ZONE|TIMESTAMP WITH TIME ZONE|

## Mapping between Oracle functions and AnalyticDB for PostgreSQL functions

|Oracle|AnalyticDB for PostgreSQL|
|------|-------------------------|
|sysdate|current timestamp|
|trunc|trunc or date trunc|
|dbms\_output.put\_line|raise|
|decode|case when or decode|
|NVL|coalesce|

## Conversion of data in PL/SQL

Procedural Language/SQL \(PL/SQL\) is a procedural extension provided by Oracle Corporation for SQL. PL/SQL enables SQL to have the features of general programming languages and can be used to implement complex business logic. PL/SQL corresponds to Procedural Language/PostgreSQL \(PL/pgSQL\) in AnalyticDB for PostgreSQL.

-   **Packages**

    PL/pgSQL does not support packages. You must convert packages to schemas. In addition, all the procedures and functions in packages must be converted to functions supported by AnalyticDB for PostgreSQL.

    Example:

    ```
    create or replace package pkg is 
    
    ...
    
    end;
    ```

    Conversion result:

    ```
    create schema pkg;
    ```

    -   Variables defined in packages

        Local variables of procedures and functions remain unchanged, and global variables can be stored in temporary tables in AnalyticDB for PostgreSQL.

    -   Package initialization blocks

        You must remove package initialization blocks. If they cannot be removed, encapsulate them in functions and call the functions when necessary.

    -   Procedures and functions defined in packages

        Convert procedures and functions defined in packages to functions supported by AnalyticDB for PostgreSQL. Each function must be defined in the schema that corresponds to the involved package.

        For example, a package named pkg has the following function:

        ```
        FUNCTION test_func (args int) RETURN int is
        var number := 10;
        BEGIN
        ... ...
        END;
        ```

        The preceding function must be converted to the following function supported by AnalyticDB for PostgreSQL:

        ```
        CREATE OR REPLACE FUNCTION pkg. test_func(args int) RETURNS int AS
        $$
        
          ... ...
        
        $$
         LANGUAGE plpgsql;
        ```

-   **Procedures and functions**

    Convert package-specific and global procedures and functions in Oracle.

    Example:

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

    Conversion result:

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

    Note the following when you convert a procedure or function:

    -   Convert the RETURN keyword to RETURNS.
    -   Use &dollar;\\$ ... &dollar;\\$ to encapsulate a function body.
    -   Pay attention to function language declarations.
    -   Convert a subprocedure to a function supported by AnalyticDB for PostgreSQL.
-   **PL statements**

    -   FOR statements

        In PL/SQL and PL/pgSQL, integer FOR loops with REVERSE statements work differently:

        -   PL/SQL counts down from the second number to the first number.
        -   PL/pgSQL counts down from the first number to the second number.
        Therefore, loop boundaries need to be exchanged during conversion. Example:

        ```
        FOR i IN REVERSE 1..3 LOOP
            DBMS_OUTPUT.PUT_LINE (TO_CHAR(i));
        END LOOP;
        ```

        Conversion result:

        ```
        FOR i IN REVERSE 3..1 LOOP
            RAISE '%' ,i;
        END LOOP;
        ```

    -   PRAGMA statements

        AnalyticDB for PostgreSQL does not support PRAGMA statements. Therefore, you must delete such statements.

    -   Transaction processing

        Functions of AnalyticDB for PostgreSQL do not support transaction control statements such as BEGIN, COMMIT, and ROLLBACK.

        These statements must be processed as follows:

        -   Delete the transaction control statements in function bodies, and put them outside the function bodies.
        -   Split functions based on COMMIT and ROLLBACK statements.
    -   EXECUTE statements

        AnalyticDB for PostgreSQL supports dynamic SQL statements that are similar to those in Oracle. The differences are as follows:

        -   The dynamic SQL statements in AnalyticDB for PostgreSQL do not support the USING syntax. You must join parameters into SQL strings.
        -   Database identifiers are packaged by using quote\_ident, and numeric values are packaged by using quote\_literal.
        Example:

        ```
        EXECUTE 'UPDATE employees_temp SET commission_pct = :x' USING a_null;
        ```

        Conversion result:

        ```
        EXECUTE 'UPDATE employees_temp SET commission_pct = ' || quote_literal(a_null);
        ```

    -   PIPE ROW statements

        Use the table functions in AnalyticDB for PostgreSQL to replace PIPE ROW statements.

        Example:

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

        Conversion result:

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

        **Note:**

        -   Convert a custom pair to a composite pair.
        -   You do not need to define the Table Of type. Replace it with Set Of in AnalyticDB for PostgreSQL.
        -   Convert a PIPE ROW statement to the following two statements:

            ```
              rec := row(i);
              return next rec;
            ```

            The preceding Oracle function can also be converted to the following statement:

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

            In the second conversion method, you do not need to pre-define the NUMSET\_T data type, which differs from the first conversion method. This requires you to specify the data type of the return value during a query, such as `select * from f1(10) as (a int, b int);`.

    -   Exception handling
        -   Use the raise function to throw an exception.
        -   After the exception is caught, the involved transaction cannot be rolled back. Rollback is only allowed outside user-defined functions.
        -   For error codes supported in AnalyticDB for PostgreSQL, visit [https://www.postgresql.org/docs/8.3/errcodes-appendix.html](https://www.postgresql.org/docs/8.3/errcodes-appendix.html).
    -   Functions with both return and out parameters

        In AnalyticDB for PostgreSQL, a function cannot contain both a return parameter and an out parameter. Therefore, you must convert the return parameter to an out parameter.

        Example:

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

        Conversion result:

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

        Then execute the `select * from test_func(1,'1') into rec;` statement to obtain the return value of the corresponding field from rec.

    -   Quotation marks \('\) contained in variables in string connections

        In the following example, the variable param2 is of the STRING data type. Assume that the value of this variable is `adb'-'pg`. If sql\_str is directly used in AnalyticDB for PostgreSQL, `-` is identified as an operator, which causes an error. Therefore, you must use the quote\_literal function to convert the variable.

        Example:

        ```
        sql_str := 'select * from test1 where col1 = ' || param1 || ' and col2 = '''|| param2 || '''and col3 = 3';
        ```

        Conversion result:

        ```
        sql_str := 'select * from test1 where col1 = ' || param1 || ' and col2 = '|| quote_literal(param2) || 'and col3 = 3';
        ```

    -   Obtain the number of days between two timestamps

        Example:

        ```
        SELECT to_date('2019-06-30 16:16:16') - to_date('2019-06-29 15:15:15') + 1 INTO v_days from dual;
        ```

        Conversion result:

        ```
        SELECT extract('days' from '2019-06-30 16:16:16'::timestamp - '2019-06-29 15:15:15'::timestamp + '1 days'::interval)::int INTO v_days;
        ```

-   **PL data types**

    -   RECORD

        Convert the RECORD data type to the composite data type in AnalyticDB for PostgreSQL.

        Example:

        ```
        TYPE rec IS RECORD (a int, b int);
        ```

        Conversion result:

        ```
        CREATE TYPE rec AS (a int, b int);
        ```

    -   NESTED TABLE
        -   As a variable in PL, the NESTED TABLE data type can be converted to the ARRAY data type in AnalyticDB for PostgreSQL.

            Example:

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

            Conversion result:

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

        -   If a nested table is used as the return value of a function, you can use the table function to replace it.
    -   ASSOCIATIVE ARRAY

        No replacement is available for this data type.

    -   VARIABLE-SIZE ARRAY

        Similar to the NESTED TABLE data type, the VARIABLE-SIZE ARRAY data type can be converted to the ARRAY data type.

    -   Global variable

        AnalyticDB for PostgreSQL does not support global variables. You can store all global variables of a package in a temporary table and define a function used to obtain them.

        Example:

        ```
        create temporary table global_variables (
                id int,
                g_count int,
                g_set_id varchar(50),
                g_err_code varchar(100)
        );
        
        insert into global_variables values(0, 1, null, null);
        
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

        In the global\_variables temporary table, the id field is the distribution key of the table. As AnalyticDB for PostgreSQL does not allow the modification of a distribution key, you must add the `tmp_rec record;` field to the table.

        To modify a global variable, execute `select * from set_variable('g_error_code', 'error'::varchar) into tmp_rec;`.

        To obtain a global variable, execute `select * from get_variable() into tmp_rec; error_code := tmp_rec.g_error_code;`.

-   **SQL**

    -   Connect By

        Connect By is used to process hierarchical queries in Oracle. No equivalent SQL statements can be found in AnalyticDB for PostgreSQL to replace a Connect By statement. You can use circular traversal by hierarchy to convert it.

        Example:

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

        Conversion result:

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
                -- Query the current hierarchical result and insert it into the tmp_curr table.
                current_result_sql := 'insert into ' || curr_tbl || ' select t1.* from ' || tablename || ' t1';
        
                if idx > 0 then
                    current_result_sql := current_result_sql || ', ' || prev_tbl || ' t2 where t1.lead_id = t2.emp_id';
                else
                    current_result_sql := current_result_sql || ' where t1.lead_id = ' || lead_id;
                end if;
                execute current_result_sql;
        
                -- If there is a loop, delete the data that has been traversed.
                if nocycle is false then
                    execute 'delete from ' || curr_tbl || ' where (lead_id, emp_id) in (select lead_id, emp_id from ' || res_tbl || ') ';
                end if;
        
                -- Exit if there is no data.
                execute 'select count(*) from ' || curr_tbl into tbl_count;
                exit when tbl_count = 0;
        
                -- Save data in the tmp_curr table to the result table.
                execute 'insert into ' || res_tbl || ' select * from ' || curr_tbl;
                execute 'truncate ' || prev_tbl;
                execute 'insert into ' || prev_tbl || ' select * from ' || curr_tbl;
                execute 'truncate ' || curr_tbl;
                idx := idx + 1;
            end loop;
        
            -- Return results.
            current_result_sql := 'select * from ' || res_tbl;
            for rec in execute current_result_sql loop
                return next rec;
            end loop;
            return;
        end
        
        $$
         language plpgsql;
        ```

    -   Rownum
        1.  This statement is used to limit the size of a result set. You can use a limit statement to replace it.

            Example:

            ```
            select * from t where rownum < 10;
            ```

            Conversion result:

            ```
            select * from t limit 10;
            ```

        2.  Use row\_number\(\) over\(\) to generate rownum.

            Example:

            ```
            select rownum, * from t;
            ```

            Conversion result:

            ```
            select row_number() over() as rownum, * from t;
            ```

    -   DUAL table
        1.  Remove dual.

            Example:

            ```
            select sysdate from dual;
            ```

            Conversion result:

            ```
            select current_timestamp;
            ```

        2.  Create a DUAL table.
    -   User-defined functions in SELECT

        AnalyticDB for PostgreSQL allows you to call user-defined functions in SELECT. However, these functions cannot contain SQL statements. If they contain SQL statements, the following error message is displayed:

        ```
        ERROR: function cannot execute on segment because it accesses relation "public.t2" (functions.c:155) (seg1 slice1 127.0.0.1:25433 pid=52153) (cdbdisp.c:1326)
        DETAIL:
        SQL statement "select b from t2 where a = $1 "
        ```

        To prevent this error message, convert the user-defined functions to SQL expressions or subqueries.

        Example:

        ```
        create or replace FUNCTION f1(arg int) RETURN int IS
            v int;
        BEGIN
            select b into v from t2 where a = arg;
            return v;
        END;
        
        select a, f1(b) from t1;
        ```

        Conversion result:

        ```
        select t1.a, t2.b from t1, t2 where t1.b = t2.a;
        ```

    -   Multi-table outer join \(+\)

        AnalyticDB for PostgreSQL does not support the \(+\) syntax. You must convert it to the standard outer join syntax.

        Example:

        ```
        oracle
        select * from a,b where a.id=b.id(+)
        ```

        Conversion result:

        ```
        select * from a left join b on a.id=b.id
        ```

        If the \(+\) syntax involves a join of three tables, use wte to join two tables first, and then perform an outer join on the wte table and the table connected with +.

        Example:

        ```
        Select * from test1 t1, test2 t2, test3 t3 where t1.col1(+) between NVL(t2.col1, t3.col1) and NVL(t3.col1, t2.col1);
        ```

        Conversion result:

        ```
        with cte as (select t2.col1 as low, t2.col2, t3.col1 as high, t3.col2 as c2 from t2, t3)
        select * from t1 right outer join cte on t1.col1 between coalesce(cte.low, cte.high) and coalesce(cte.high,cte.low);
        ```

    -   Merge Into

        To convert the Merge Into syntax, execute an UPDATE statement in AnalyticDB for PostgreSQL first, and use the `GET DIAGNOSTICS rowcount := ROW_COUNT;` statement to obtain the number of updated rows. If the number of updated rows is 0, execute an INSERT statement to insert data.

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

        Conversion result:

        ```
        Update test1 t1 SET t1.col1 = test2.col1+1, test3.col2 = S.col2+2 where test2.col1 = 1 and test2.col2 = 2;
        GET DIAGNOSTICS rowcount := ROW_COUNT;
        if rowcount = 0 then
            insert into test1 values(test2.col1+1, test3.col2+2);
        end if;
        ```

    -   Sequence

        Example:

        ```
        create sequence seq1;
        select seq1.nextval from dual;
        ```

        Conversion result:

        ```
        create SEQUENCE seq1;
        select nextval('seq1');
        ```

    -   Cursor
        -   In Oracle, you can use the following statement to traverse cursors.

            Example:

            ```
            FUNCTION test_func() IS
                Cursor data_cursor IS SELECT * from test1;
            BEGIN
                FOR I IN data_cursor LOOP
                    Do something with I;
            END LOOP;
            END;
            ```

            Conversion result:

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

        -   In Oracle, cursors with the same name can be opened in recursively called functions. However, this is not allowed in AnalyticDB for PostgreSQL. Equivalent statements in AnalyticDB for PostgreSQL must be in the format of For I in query.

            Example:

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

            Conversion result:

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


