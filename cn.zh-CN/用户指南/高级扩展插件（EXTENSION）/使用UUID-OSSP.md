# 使用UUID-OSSP {#concept_2005670 .concept}

本文介绍如何使用UUID-OSSP。

## 安装UUID-OSSP {#section_4f3_scd_12j .section}

安装UUID-OSSP代码如下所示：

``` {#codeblock_qd6_lyw_oue}
CREATE EXTENSION "uuid-ossp";
```

**说明：** 只有superuser或rds\_superuser权限用户可以安装该插件。

## 插件说明 {#section_9cv_xdi_4p9 .section}

`"uuid-ossp"` extension创建后，会自动创建`uuid`数据类型且支持btree索引。

``` {#codeblock_kr1_uks_s7i}
mydb=> create extension "uuid-ossp";
CREATE EXTENSION
mydb=> \dT
     List of data types
 Schema | Name | Description
--------+------+-------------
 public | uuid |
(1 row)
```

UUID数据类型用来存储RFC 4122、ISO/IEF 9834-8:2005以及相关标准定义的通用唯一标识符（UUID）。（部分系统认为这个数据类型为全球唯一标识符，或GUID。） 这个标识符是一个由算法产生的128位标识符，使它不可能在已知使用相同算法的模块中和其他方式产生的标识符相同。 因此，对分布式系统而言，这种标识符比序列能更好的提供唯一性保证，因为序列只能在单一数据库中保证唯一。

UUID被写成一个小写十六进制数字的序列，由分字符分成几组， 特别是一组8位数字+3组4位数字+一组12位数字，总共32个数字代表128位， 标准的UUID示例如下：

``` {#codeblock_ch3_say_k29}
a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11
```

同样支持以其他方式输入：大写数字， 由花括号包围的标准格式，省略部分或所有连字符，在任意一组四位数字之后加一个连字符。如：

``` {#codeblock_beq_qyn_r67}
A0EEBC99-9C0B-4EF8-BB6D-6BB9BD380A11
{a0eebc99-9c0b-4ef8-bb6d-6bb9bd380a11}
a0eebc999c0b4ef8bb6d6bb9bd380a11
a0ee-bc99-9c0b-4ef8-bb6d-6bb9-bd38-0a11
{a0eebc99-9c0b4ef8-bb6d6bb9-bd380a11}
```

**说明：** 当前版本不支持uuid类型字段作为分布键。

## 插件函数说明 {#section_t4b_uj7_hi2 .section}

-   UUID生成函数

    |函数|描述|
    |--|--|
    |`uuid_generate_v1()`|此函数会生成v1版本的UUID。算法使用了计算机的MAC地址和时间戳。 **说明：** 该UUID会泄露计算机标识和生成时间，不适合对安全性要求较高的应用。

 |
    |`uuid_generate_v1mc()`|此函数会生成一个v1版本的UUID。和`uuid_generate_v1()`的区别在于`uuid_generate_v1mc()`使用的是一个随机多播MAC地址，`uuid_generate_v1()`使用的是计算机的真实的MAC地址。|
    |`uuid_generate_v3(namespace uuid, name text)`|此函数会生成一个v3版本的UUID。这个函数会使用指定输入名称`name`在指定的命名空间`namespace` 中生成。     -   指定的命名空间应该是调用下表中的函数`uuid_ns_*()`返回的常量。
    -   参数`name`是一个指定命名空间`namespace`中的标识符。
 例如：     ``` {#codeblock_aac_4d8_e0w}
SELECT uuid_generate_v3(uuid_ns_url(), 'http://www.postgresql.org');
    ```

 `name`会使用MD5算法进行哈希，从产生的UUID中不能反向获得明文。利用这个方法生成的UUID不需要随机算法且不依赖任何运行相关的环境因素，生成过程是可重复的。

 |
    |`uuid_generate_v4()`|此函数会生成一个v4版本的UUID。算法完全依靠随机数。|
    |`uuid_generate_v5(namespace uuid, name text)`|此函数会生成一个v5版本的UUID。工作过程类似于v3版本的 UUID，但是v5版本使用的的是SHA-1的哈希算法，因为SHA-1算法被认为比 MD5算法更安全，所有应该尽量使用v5版本而不是v3版本。|

-   返回 UUID 常量的函数

    |函数|描述|
    |--|--|
    |`uuid_nil()`|代表nil的UUID常量，此处不应该看作一个真正的UUID。|
    |`uuid_ns_dns()`|代表DNS命令空间的UUID常量。|
    |`uuid_ns_url()`|代表URL命名空间的UUID常量。|
    |`uuid_ns_oid()`|代表ISO对象标识符（OID）命名空间的UUID常量。 **说明：** 此处OID是ASN.1的OID，和PostgreSQL用的OID没有关系。

 |
    |`uuid_ns_x500()`|代表X.500识别名字（DN）命名空间的UUID常量。|


## 示例 {#section_f2y_dvo_dix .section}

``` {#codeblock_f8k_pmn_u22}
mydb=# create extension "uuid-ossp";
CREATE EXTENSION
mydb=# SELECT uuid_generate_v1();
           uuid_generate_v1
--------------------------------------
 c7f83ba4-bd93-11e9-8674-40a8f01ec4e8
(1 row)

mydb=# SELECT uuid_generate_v3(uuid_ns_url(), 'http://www.postgresql.org');
           uuid_generate_v3
--------------------------------------
 cf16fe52-3365-3a1f-8572-288d8d2aaa46
(1 row)

mydb=# SELECT uuid_generate_v4();
           uuid_generate_v4
--------------------------------------
 d7a8d47e-58e3-4bd9-9340-8553ac03d144
(1 row)

mydb=# SELECT uuid_generate_v5(uuid_ns_url(), 'http://www.postgresql.org');
           uuid_generate_v5
--------------------------------------
 e1ee1ad4-cd4e-5889-962a-4f605a68d94e
(1 row)

mydb=# create table x(id uuid, value float4) distributed by (value);
CREATE TABLE
mydb=# insert into x select uuid_generate_v4(),(r*random()) from generate_series(1,100000)r;
INSERT 0 100000
mydb=# CREATE INDEX idx_x_id ON x USING btree (id);
CREATE INDEX
mydb=# ANALYZE x;
ANALYZE
mydb=# explain SELECT count(1) from x where id = '9f830fc9-a1ee-425f-844b-9c73290a91ad';
                                      QUERY PLAN
--------------------------------------------------------------------------------------
 Aggregate  (cost=200.44..200.45 rows=1 width=8)
   ->  Gather Motion 3:1  (slice1; segments: 3)  (cost=200.37..200.42 rows=1 width=8)
         ->  Aggregate  (cost=200.37..200.38 rows=1 width=8)
               ->  Index Scan using idx_x_id on x  (cost=0.00..200.37 rows=1 width=0)
                     Index Cond: id = '9f830fc9-a1ee-425f-844b-9c73290a91ad'::uuid
 Settings:  enable_seqscan=off; optimizer=off
 Optimizer status: legacy query optimizer
(7 rows)
```

## 参考文档 {#section_a50_hzn_zkm .section}

-   [http://www.ossp.org/pkg/lib/uuid/](http://www.ossp.org/pkg/lib/uuid/)
-   [https://www.postgresql.org/docs/9.4/uuid-ossp.html](https://www.postgresql.org/docs/9.4/uuid-ossp.html)
-   [http://www.postgres.cn/docs/9.4/uuid-ossp.html](http://www.postgres.cn/docs/9.4/uuid-ossp.html)

