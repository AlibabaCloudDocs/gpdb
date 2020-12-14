# Compatibility comparisons between AnalyticDB for PostgreSQL V4.3 and V6.0

This topic describes the compatibility comparisons between AnalyticDB for PostgreSQL V4.3 and V6.0. If you upgrade your AnalyticDB for PostgreSQL instances from V4.3 to V6.0, you must make necessary adjustments.

## Query optimizers

|Item|V4.3|V6.0|
|----|----|----|
|Default query optimizer|Legacy|ORCA|

Both AnalyticDB for PostgreSQL V4.3 and V6.0 support Legacy and ORCA query optimizers. For more information about these query optimizers, see [Choose a query optimizer](/intl.en-US/Best Practices/Optimize query performance.md).

## Escape characters

-   In AnalyticDB for PostgreSQL V6.0, `backslashes (\)` within strings are not used as escape characters.
-   You can execute the following statement to use backslashes \(\\\) as escape characters. However, we recommend that you do not perform this operation.

    ```
    set standard_conforming_strings = off;
    ```

    **Note:** The preceding statement is available only for sessions. If you want to use this statement for AnalyticDB for PostgreSQL instances, [submit a ticket](https://workorder.console.aliyun.com/console.htm#/ticket/add?productCode=gpdb) and request technical support of AnalyticDB for PostgreSQL to configure the escape characters based on your requirements.


## Data type conversion

-   AnalyticDB for PostgreSQL V6.0 does not automatically convert a string of the `YYYYMMDDHH24MISS` format to a timestamp. To perform such conversion, you can use the `to_timestamp/to_char` built-in function.``
-   Compared with V4.3, AnalyticDB for PostgreSQL V6.0 does not implicitly convert numeric data types to TEXT. After you upgrade AnalyticDB for PostgreSQL from V4.3 to V6.0, you must add functions to the original SQL statements to convert numeric data types to TEXT. Examples:

    ```
    create or replace function substr(numeric, integer,integer)returns text as $$
    select substr($1::text,$2,$3);
    $$ language sql IMMUTABLE strict;
    ```

    ```
    create or replace function pg_catalog.btrim(str numeric) returns text as $$
    return $_[0];
    $$ language plperl IMMUTABLE strict;
    ```

    ```
    create or replace function to_date(timestamp, text) returns date as $$
    select to_date($1::text,$2);
    $$ language sql IMMUTABLE strict;
    ```

    ```
    create or replace function to_date(integer, text) returns date as $$
    select to_date($1::text,$2);
    $$ language sql IMMUTABLE strict;
    ```

-   You must manually rewrite the SQL statements or functions in which the data type needs to be implicitly converted to TEXT.

## Error logs of external tables

In AnalyticDB for PostgreSQL V6.0, you cannot use the `INTO error_table` clause in the `CREATE EXTERNAL TABLE` or `COPY` statement. Instead, you can use a built-in function to manipulate error logs of external tables.``

```
gp_read_error_log('$external_table')
gp_truncate_error_log('$external_table')
```

## Data types

-   If you change the storage format of NUMERIC files, the corresponding disk space is affected.
-   If you change the MONEY data type from 32-bit to 64-bit, the corresponding disk space is affected.
-   In AnalyticDB for PostgreSQL V6.0, you cannot use the following data types for distribution keys:
    -   abstime
    -   reltime
    -   tinterval
    -   money
    -   anyarray

## Keywords

AnalyticDB for PostgreSQL V6.0 adds, modifies, and deletes some keywords. Names of database objects cannot be the same as keywords.

The use of keywords varies between different categories. You can execute the following statement to view all keywords and their categories in both AnalyticDB for PostgreSQL V4.3 and V6.0:

```
select * from pg_get_keywords();
```

|Category Code|Category|Description|
|-------------|--------|-----------|
|U|unreserved|Unreserved. The keywords of this category can be used as names for all objects including views, tables, functions, indexes, fields, and types.|
|C|unreserved \(cannot be function or type name\)|Unreserved. The keywords of this category can be used as names of objects except for functions and types.|
|T|reserved \(can be function or type name\)|Reserved. The keywords of this category cannot be used as names of objects except for functions and types.|
|R|reserved|Reserved. The keywords of this category cannot be used as names of objects.|

|Keyword|V4.3|V6.0|
|-------|----|----|
|between|reserved|unreserved \(cannot be function or type name\)|
|collation|None|reserved \(can be function or type name\)|
|concurrently|unreserved|reserved \(can be function or type name\)|
|convert|unreserved \(cannot be function or type name\)|None|
|filter|reserved|unreserved|
|lateral|N/A|reserved|
|new|reserved|None|
|off|reserved|unreserved|
|old|reserved|None|
|percentile\_cont|unreserved \(cannot be function or type name\)|None|
|percentile\_disc|unreserved \(cannot be function or type name\)|None|
|range|reserved|unreserved|
|reindex|unreserved|reserved|
|rows|reserved|unreserved|
|sort|reserved|reserved \(can be function or type name\)|
|variadic|None|reserved|

## System tables

Some system tables differentiate between AnalyticDB for PostgreSQL V4.3 and V6.0. If your business logic references the following system tables, you must modify the referenced system tables to avoid errors.

|V4.3|V6.0|Description|
|----|----|-----------|
|pg\_class.reltoastidxid|None|AnalyticDB for PostgreSQL V6.0 does not support this table.|
|pg\_stat\_activity.procpid|pg\_stat\_activity.pid|In AnalyticDB for PostgreSQL V6.0, the column name is changed from procpid to pid.|
|pg\_stat\_activity.current\_query|pg\_stat\_activity.statepg\_stat\_activity.query

|AnalyticDB for PostgreSQL V6.0 displays the current activity of a server process in two columns. The state column displays the current overall state of the backend. The query column displays the currently executing query.|
|gp\_distribution\_policy.attrnums|gp\_distribution\_policy.distkey|In AnalyticDB for PostgreSQL V6.0, the column name is changed from attrnums to distkey, and the data type of this column is changed to int2vector.|
|sesion\_level\_memory\_consumption.\_\_gp\_localidsesion\_level\_memory\_consumption.\_\_gp\_masterid

|None|AnalyticDB for PostgreSQL V6.0 does not support these tables.|
|pg\_filespacepg\_filespace\_entry

|None|AnalyticDB for PostgreSQL V6.0 does not support these tables.|

## Parameters of built-in functions

AnalyticDB for PostgreSQL V6.0 changes the parameters of some built-in functions.

|V4.3|V6.0|Description|
|----|----|-----------|
|int4\_avg\_accum\(bytea, integer\)|int4\_avg\_accum\(bigint\[\], integer\)|None|
|string\_agg\(expression\)|string\_agg\(expression, delimiter\)|In AnalyticDB for PostgreSQL V6.0, you can use the string\_agg function to convert an expression to a string.|

## Other comparisons

|Item|V4.3|V6.0|
|----|----|----|
|LEFT\(\) function|Not supported|Supported|
|UPDATE operations on distribution key columns|Not supported|Supported|

