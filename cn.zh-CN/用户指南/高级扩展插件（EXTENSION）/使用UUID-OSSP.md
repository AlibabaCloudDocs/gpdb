# 使用UUID-OSSP {#concept_2005670 .concept}

本文介绍如何使用UUID-OSSP。

## 安装UUID-OSSP {#section_4f3_scd_12j .section}

安装UUID-OSSP代码如下所示：

``` {#codeblock_qd6_lyw_oue}
CREATE EXTENSION "uuid-ossp";
```

**说明：** 只有superuser或rds\_superuser权限用户可以安装该插件。

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
    |`uuid_generate_v5(namespace uuid, name text)`|此函数会生成一个v5版本的UUID。工作过程类似于v3版本的 UUID，但是v5版本使用的的是SHA-1的哈希算法，因为SHA-1算法被认为比 MD5算法更安全，所有应该尽量使用版本 5 而不版本 3。|

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
```

## 参考文档 {#section_a50_hzn_zkm .section}

-   [https://www.postgresql.org/docs/9.4/uuid-ossp.html](https://www.postgresql.org/docs/9.4/uuid-ossp.html)
-   [http://www.postgres.cn/docs/9.4/uuid-ossp.html](http://www.postgres.cn/docs/9.4/uuid-ossp.html)

