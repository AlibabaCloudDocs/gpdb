# TPC-B

AnalyticDB for PostgreSQL 6.0 \(简称ADBPG6.0\)在事务处理性能上相比上个版本ADBPG4.3有了质的飞跃，本文将以TPC-B业界标准事务性能测试benchmark来展示ADBPG6.0在事务上的处理能力。

## TPC-B简介

TPC-B是由TPC\(Transaction Processing Performance Council，事务处理性能委员会\)提供的benchmark，主要用于衡量一个系统每秒能够处理的并发事务数。TPC-B不像TPC-C那样模拟了现实生活中一个具体的交易场景，其中的事务都是由简单SQL构成的没有语义的事务（事务中混杂了大表与小表的插入、更新与查询操作），而且每个client的请求间也不会像TPC-C那样会有一个human think time的间隔时间，而是一旦前一个事务执行完成，立马会有下一个事务请求发出。因此，TPC-B经常用于对数据库系统的事务性能压测。TPC-B性能的衡量指标是每秒处理的事务数量，即TPS（Transactions per Second）。

## 测试环境

-   测试数据

使用PostgreSQL提供的开源工具pgbench来生成TPC-B的测试数据，填充因子为100，比例因子为11424。因此，最后TPC-B各个表的数据量如下：

|表名|数据量（行数）|
|--|-------|
|pgbench\_accounts|1142400000|
|pgbench\_branches|11424|
|pgbench\_history|0|
|pgbench\_tellers|114240|

-   测试集群

测试的ADBPG6.0集群由16个计算节点，每个计算节点有4个CPU核，32GB内存，存储类型为高性能SSD，具体规格选用参考：[规格及选型](/intl.zh-CN/规格和定价/规格及选型.md)。另外，在同一个域里面建了一个ECS（放在与ADBPG实例相同的VPC中），在上面运行pgbench工具来生成TPC-B负载，向ADBPG发送请求。

-   集群配置参数

需要获取极致的TP性能，需要对一下参数进行修改。其中有些参数用户无法自行设置，请联系ADBPG值班人员进行修改。

|参数|说明|
|--|--|
|set optimizer = off|关闭ADBPG针对AP场景的orca优化器，对TP性能更友好。|
|set shared\_buffers = 8GB|将ADBPG的数据共享缓存调大。修改该参数需要重启实例。|
|set wal\_buffers = 256MB|将ADBPG的WAL日志缓存调大。修改该参数需要重启实例。|
|set log\_statement = none|将日志输出关闭。|
|set random\_page\_cost = 10|将随机访问代价开销调小，有利于查询走索引。|
|set gp\_resqueue\_priority = off set resource\_scheduler = off 

|将ADBPG的resource queue关闭。需要重启实例|

## 测试结果

测试在不同并发度情况下，ADBPG6.0的性能结果，结果如下图所示，其中横坐标代表不同的并发数，纵坐标代表测试性能（单位为TPS，transactions per second）。除了对ADBPG的TPC-B的性能测试之外，还使用pgbench对只读、只更新和只插入进行了测试。从下面的测试性能结果图中可以看出，ADBPG的TPC-B峰值性能可以达到 5923 TPS，只读峰值性能为 150084 TPS， 只更新峰值性能为31023 TPS，只插入的峰值性能为 60367 TPS。

-   TPC-B性能

|并发|1|8|16|32|48|64|96|128|168|192|
|--|--|--|--|--|--|--|--|---|---|---|
|TPS|199|1481|2031|2568|2795|3236|3622|5923|5037|5211|
|RT\(ms\)|5.02|5.4|7.88|12.5|17.2|19.8|26.5|21.6|33.3|36.8|

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2734032951/p87765.png)

-   只读性能

|并发|1|8|16|32|48|64|96|128|160|192|
|--|--|--|--|--|--|--|--|---|---|---|
|TPS|1875|14226|26784|51115|72767|92370|125708|143297|150084|139637|
|RT\(ms\)|0.53|0.56|0.6|0.63|0.66|0.69|0.76|0.89|1.07|1.37|

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2734032951/p87767.png)

-   只更新性能

|并发|1|8|16|32|48|64|96|128|160|192|
|--|--|--|--|--|--|--|--|---|---|---|
|TPS|787|6486|12127|19333|21083|24199|31023|27464|24362|23600|
|RT\(ms\)|1.27|1.23|1.32|1.66|2.28|2.64|3.09|4.66|6.57|8.14|

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/2734032951/p87770.png)

-   只插入性能

|并发|1|8|16|32|48|64|96|128|160|192|
|--|--|--|--|--|--|--|--|---|---|---|
|TPS|1497|11463|21771|40543|56887|59967|60367|59365|53375|48927|
|RT\(ms\)|0.69|0.7|0.73|0.79|0.84|1.07|1.59|2.16|3.0|3.9|

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3734032951/p87771.png)

## 如何使用pgbench进行ADBPG的TPC-B测试

-   工具下载安装

有两种方式对pgbench工具进行安装：

1.  源码安装：下载开源数据库PostgreSQL源码，然后到pgbench对应的目录中单独对pgbench进行编译生成可执行的二进制文件。
2.  二进制安装：可以先直接yum install postgresql-server来安装PostgreSQL程序，此过程会自动安装pgbench工具。

-   数据加载

执行如下命令，会自动将数据生成加载到ADBPG实例中。

```
# 其中-F参数就是上文说的装填因子，-s值得是比例因子
./pgbench -i -F 100 -s 11424 -p port -h con_addr -U user_name db_name
```

-   执行测试

执行TPC-B负载

```
# -c指定了连接数据库client数量，-j指定了建立连接使用的线程数量，推荐将两者设置成一样
# -T指定了测试执行时间，单位为秒
./pgbench -h con_addr -p port -r -n -c 96 -j 96 -T 120 -f all.sql -U user_name db_name
```

其中all.sql为TPC-B负载，其内容如下：

```
\set scale 11424
\set nbranches 1 * :scale
\set ntellers 10 * :scale
\set naccounts 100000 * :scale
\setrandom aid 1 :naccounts
\setrandom bid 1 :nbranches
\setrandom tid 1 :ntellers
\setrandom delta -5000 5000

BEGIN;
 UPDATE pgbench_accounts SET abalance = abalance + :delta WHERE aid = :aid;
 SELECT abalance FROM pgbench_accounts WHERE aid = :aid;
 UPDATE pgbench_tellers SET tbalance = tbalance + :delta WHERE tid = :tid;
 UPDATE pgbench_branches SET bbalance = bbalance + :delta WHERE bid = :bid;
 INSERT INTO pgbench_history (tid, bid, aid, delta, mtime) VALUES (:tid, :bid, :aid, :delta, CURRENT_TIMESTAMP);
END;
```

-   执行只读负载

```
# -c指定了连接数据库client数量，-j指定了建立连接使用的线程数量，推荐将两者设置成一样
# -T指定了测试执行时间，单位为秒
./pgbench -h con_addr -p port -r -n -c 96 -j 96 -T 120 -f select.sql -U user_name db_name
```

其中select.sql为只读负载，其内容如下：

```
\set scale 11424
\set naccounts 100000 * :scale
\setrandom aid 1 :naccounts

SELECT abalance FROM pgbench_accounts WHERE aid = :aid;
				
```

-   执行只更新负载

```
# -c指定了连接数据库client数量，-j指定了建立连接使用的线程数量，推荐将两者设置成一样
# -T指定了测试执行时间，单位为秒
./pgbench -h con_addr -p port -r -n -c 96 -j 96 -T 120 -f update.sql -U user_name db_name
```

其中update.sql为只更新负载，其内容如下：

```
\set scale 11424
\set naccounts 100000 * :scale
\setrandom aid 1 :naccounts
\setrandom delta -5000 5000
UPDATE pgbench_accounts SET abalance = abalance + :delta WHERE aid = :aid;
```

-   执行只插入负载

```
# -c指定了连接数据库client数量，-j指定了建立连接使用的线程数量，推荐将两者设置成一样
# -T指定了测试执行时间，单位为秒
./pgbench -h con_addr -p port -r -n -c 96 -j 96 -T 120 -f insert.sql -U user_name db_name
```

其中insert.sql为只插入负载，其内容如下：

```
\set scale 11424
\set nbranches 1 * :scale
\set ntellers 10 * :scale
\set naccounts 100000 * :scale
\setrandom aid 1 :naccounts
\setrandom bid 1 :nbranches
\setrandom tid 1 :ntellers
\setrandom delta -5000 5000
INSERT INTO pgbench_history (tid, bid, aid, delta, mtime) VALUES (:tid, :bid, :aid, :delta, CURRENT_TIMESTAMP);
```

