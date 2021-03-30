# Use HyperLogLog

Alibaba Cloud Database AnalyticDB for PostgreSQL is deeply optimized. In addition to the native Greenplum Database function, HyperLogLog is also supported to provide solutions for Internet advertisement analysis and industries with similar estimation analysis and computing requirements. In order to quickly estimate the PV, UV and other business indicators.

## Create a HyperLogLog plug-in

Execute the following command to create HyperLogLog plug-in:

```
CREATE EXTENSION hll;
```

## Basic types

-   Execute the following statement to create a table containing the hll field:

    ```
    create table agg (id int primary key,userids hll);
    ```

-   Run the following command to convert int to hll\_hashval:

    ```
    select 1::hll_hashval;
    ```


## Basic operators

-   The hll type supports=,!=, =, <\>, \| \|, and\#.

    ```
    select hll_add_agg(1::hll_hashval) = hll_add_agg(2::hll_hashval);
    select hll_add_agg(1::hll_hashval) || hll_add_agg(2::hll_hashval);
    select #hll_add_agg(1::hll_hashval);
    ```

-   The hll\_hashval type supports=,!=, =and <\>.

    ```
    select 1::hll_hashval = 2::hll_hashval;
    select 1::hll_hashval <> 2::hll_hashval;
    ```


## Basic functions

-   hll\_hash\_boolean, hll\_hash\_smallint, and hll\_hash\_bigint.

    ```
    select hll_hash_boolean(true);
    select hll_hash_integer(1);
    ```

-   hll\_add\_agg: converts the int format to the hll format.

    ```
    select hll_add_agg(1::hll_hashval);
    ```

-   hll\_union: Union of hll.

    ```
    select hll_union(hll_add_agg(1::hll_hashval),hll_add_agg(2::hll_hashval));
    ```

-   hll\_set\_defaults: sets the precision.

    ```
    select hll_set_defaults(15,5,-1,1);
    ```

-   hll\_print: displays debug information.

    ```
    select hll_print(hll_add_agg(1::hll_hashval));
    ```


## Examples

```
create table access_date (acc_date date unique, userids hll);
insert into access_date select current_date, hll_add_agg(hll_hash_integer(user_id)) from generate_series(1,10000) t(user_id);
insert into access_date select current_date-1, hll_add_agg(hll_hash_integer(user_id)) from generate_series(5000,20000) t(user_id);
insert into access_date select current_date-2, hll_add_agg(hll_hash_integer(user_id)) from generate_series(9000,40000) t(user_id);
postgres=# select #userids from access_date where acc_date=current_date;
     ? column?
------------------
 9725.85273370708
(1 row)
postgres=# select #userids from access_date where acc_date=current_date-1;
     ? column?
------------------
 14968.6596883279
(1 row)
postgres=# select #userids from access_date where acc_date=current_date-2;
     ? column?
------------------
 29361.5209149911
(1 row)
```

