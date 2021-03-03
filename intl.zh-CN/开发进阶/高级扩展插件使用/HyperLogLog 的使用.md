# HyperLogLog 的使用

阿里云深度优化云数据库 AnalyticDB for PostgreSQL，除原生 Greenplum Database 功能外，还支持 HyperLogLog，为互联网广告分析及有类似预估分析计算需求的行业提供解决方案，以便于快速预估 PV、UV 等业务指标。

## 创建 HyperLogLog 插件

执行如下命令，创建 HyperLogLog 插件：

```
CREATE EXTENSION hll;
```

## 基本类型

-   执行如下命令，创建一个含有 hll 字段的表：

    ```
    create table agg (id int primary key,userids hll);
    ```

-   执行如下命令，进行 int 转 hll\_hashval：

    ```
    select 1::hll_hashval;
    ```


## 基本操作符

-   hll 类型支持 =、!=、<\>、\|\| 和 \#。

    ```
    select hll_add_agg(1::hll_hashval) = hll_add_agg(2::hll_hashval);
    select hll_add_agg(1::hll_hashval) || hll_add_agg(2::hll_hashval);
    select #hll_add_agg(1::hll_hashval);
    ```

-   hll\_hashval 类型支持 =、!= 和 <\>。

    ```
    select 1::hll_hashval = 2::hll_hashval;
    select 1::hll_hashval <> 2::hll_hashval;
    ```


## 基本函数

-   hll\_hash\_boolean、hll\_hash\_smallint 和 hll\_hash\_bigint 等 hash 函数。

    ```
    select hll_hash_boolean(true);
    select hll_hash_integer(1);
    ```

-   hll\_add\_agg：可以将 int 转 hll 格式。

    ```
    select hll_add_agg(1::hll_hashval);
    ```

-   hll\_union：hll 并集。

    ```
    select hll_union(hll_add_agg(1::hll_hashval),hll_add_agg(2::hll_hashval));
    ```

-   hll\_set\_defaults：设置精度。

    ```
    select hll_set_defaults(15,5,-1,1);
    ```

-   hll\_print：用于 debug 信息。

    ```
    select hll_print(hll_add_agg(1::hll_hashval));
    ```


## 示例

```
create table access_date (acc_date date unique, userids hll);
insert into access_date select current_date, hll_add_agg(hll_hash_integer(user_id)) from generate_series(1,10000) t(user_id);
insert into access_date select current_date-1, hll_add_agg(hll_hash_integer(user_id)) from generate_series(5000,20000) t(user_id);
insert into access_date select current_date-2, hll_add_agg(hll_hash_integer(user_id)) from generate_series(9000,40000) t(user_id);
postgres=# select #userids from access_date where acc_date=current_date;
     ?column?
------------------
 9725.85273370708
(1 row)
postgres=# select #userids from access_date where acc_date=current_date-1;
     ?column?
------------------
 14968.6596883279
(1 row)
postgres=# select #userids from access_date where acc_date=current_date-2;
     ?column?
------------------
 29361.5209149911
(1 row)
```

