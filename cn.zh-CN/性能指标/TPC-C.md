# TPC-C

AnalyticDB for PostgreSQL 6.0 \(简称ADBPG6.0\)在事务处理性能上相比上个版本ADBPG4.3有了质的飞跃，本文将以TPC-C业界标准事务性能测试benchmark来展示ADBPG6.0在事务上的处理能力。

## TPC-C简介

TPC-C是由TPC\(Transaction Processing Performance Council，事务处理性能委员会\)提供的专门针对联机交易处理系统的规范，TPC-C模拟的是一个大型的商品批发销售公司交易负载。这个事务负载主要由9张表组成，主要涉及5类交易类型：新订单生成（New-Order）、订单支付（Payment）、发货（Delivery）、订单状态查询（Order-Status）、和库存状态查询（Stock-Level）。TPC-C测试使用吞吐量指标（Transaction per minute，简称tpmC\)来衡量系统的性能，其中所统计的事务指的是新订单生成的事务，即以每分钟新订单生成的事务数来衡量系统的性能指标（在标准的TPC-C测试中，新订单的事务数量占总事务数的45%左右）。

## 测试环境

-   测试数据

使用开源的benchmarksql工具生成1000个warehouse的数据集，各个表的数据量如下表所示:

|表名|数据量（行数）|
|--|-------|
|bmsql\_warehouse|1000|
|bmsql\_district|10000|
|bmsql\_customer|30000000|
|bmsql\_history|30000000|
|bmsql\_new\_order|9000000|
|bmsql\_oorder|30000000|
|bmsql\_order\_line|299976737|
|bmsql\_item|100000|
|bmsql\_stock|100000000|

-   测试集群

测试的ADBPG6.0集群由16个计算节点，每个计算节点有4个CPU核，32GB内存，存储类型为高性能SSD，具体规格选用参考：[规格及选型](/cn.zh-CN/规格和定价/规格及选型.md)。

另外在同一个域里面建了一个ECS（放在与测试ADBPG实例相同的VPC中），然后在这个ECS上运行benchmarksql工具来生成TPC-C负载，向ADBPG实例发送请求。

-   集群配置参数

要获取极致的TP性能，需要对以下集群参数进行修改。其中有些参数用户无法自行设置，请联系ADBPG值班人员进行修改。

|参数|说明|
|--|--|
|set optimizer = off|关闭ADBPG针对AP场景的orca优化器，对TP性能更友好。|
|set shared\_buffers = 8GB|将ADBPG的数据共享缓存调大。修改该参数需要重启实例|
|set wal\_buffers = 256MB|将ADBPG的WAL日志缓存调大。修改该参数需要重启实例。|
|set log\_statement = none|将日志输出关闭。|
|set random\_page\_cost = 10|将随机访问代价开销调小，有利于查询走索引。|
|set gp\_resqueue\_priority = off set resource\_scheduler = off 

|将ADBPG的resource queue关闭。需要重启实例。|

## 测试结果

测试在不同并发度情况下，ADBPG6.0的性能结果，结果如下图所示，其中横坐标代表不同的并发数，纵坐标代表TPC-C性能（单位为tpmC），从图中可以看到，ADBPG6.0的峰值性能可以达到 101231.3 tpmC。

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/2734032951/p87912.png)

## 如何使用benchmarksql进行ADBPG的TPC-C测试

-   工具下载

使用ADBPG提供的[benchmarksql](https://yq.aliyun.com/go/articleRenderRedirect?url=https%3A%2F%2Fgithub.com%2Fdreamedcheng%2Fbenchmarksql-5.0)，相比于开源版本，差异在于：1、由于ADBPG是分布式数据库，在建表语句时指定了分布键，一方面可以使得数据分布更加均匀，另一方面使得两阶段事务涉及计算节点数更少以便减少分布式事务的通信开销。2、修复了在生成csv数据集的时候，数据字段与建表字段不匹配的问题。

-   数据生成加载

1.  生成数据

使用如下命令可以生成TPC-C中各个表的csv数据，然后可以使用COPY方式将数据加载到数据库中。

```
# adbpg.properties为配置文件
./runLoader.sh adbpg.properties
```

在执行前，需要修改adbpg.properties中的如下内容：

```
db=postgres // 使用默认的postgres
driver=org.postgresql.Driver // 使用默认的org.postgresql.Drive
conn=jdbc:postgresql://xxxxx.xxxx.xxxx.rds.aliyuncs.com:3432/benchmarksql //adbpg实例连接地址
user=tpcc_test // 连接adbpg实例测试的用户
password=your_password // 对应用户的密码

warehouses=1000 // 指定生成数据集warehouse的数量
loadWorkers=16  // 指定生成数据的线程数量，如果CPU core和内存够用可以将其大小调大，这样速度更快
fileLocation=/data/tpcc_1000w/ // 指定生成数据的存储目录
```

2. 数据加载

\(1\) 建表: 执行benchmarksql工具提供的tableCreates.sql脚本。

\(2\) 数据加载: 使用copy命令将上节中生成的csv数据导入到ADBPG对应的表中。

```
# 使用COPY方式将数据加载到数据库中
psql -h conn_addr -d benchmarksql -p your_port -U tpcc_test -c "\copy bmsql_history from '/data/tpcc_1000w/cust-hist.csv' with delimiter ',' null '';"
```

\(3\) 创建索引: 执行benchmarksql工具提供的indexCreates.sql脚本。

\(4\) 执行analyze: 数据加载完并创建好索引后，需要对所有表执行analyze，以便ADBPG的优化器能做出好的执行计划。

-   执行测试

执行benchmarksql的如下脚本, 其中adbpg.properties为配置文件

```
./runBenchmark.sh adbpg.properties
```

在执行前，需要修改adbpg.properties中的如下内容

```
db=postgres // 使用默认的postgres
driver=org.postgresql.Driver // 使用默认的org.postgresql.Drive
conn=jdbc:postgresql://xxxxx.xxxx.xxxx.rds.aliyuncs.com:3432/benchmarksql //adbpg实例连接地址
user=tpcc_test // 连接adbpg实例测试的用户
password=your_password // 对应用户的密码

terminals=128 // TPC-C测试的并发数
//To run specified transactions per terminal- runMins must equal zero
runTxnsPerTerminal=0 // 使用默认的0
//To run for specified minutes- runTxnsPerTerminal must equal zero
runMins=10 // TPC-C执行时间，单位分钟
//Number of total transactions per minute
limitTxnsPerMin=30000000 // 使用默认的30000000

// 下面配置各个事务的比重，也使用如下给定的默认值
//The following five values must add up to 100
//The default percentages of 45, 43, 4, 4 & 4 match the TPC-C spec
newOrderWeight=45
paymentWeight=43
orderStatusWeight=4
deliveryWeight=4
stockLevelWeight=4
```

运行上面的脚本后，就会在屏幕上实时输出数据库当前处理的事务总数，等运行完runMins指定的时间后，屏幕就会输出如下所示的数据库TPC-C性能的tpmC数值：

Term-00, Running Average tpmTOTAL: 225114.24    Current tpmTOTAL: 7445652    Memory Usage: 116MB / 168MB18:28:56,388 \[Thread-127\] INFO   jTPCC : Term-00,18:28:56,388 \[Thread-127\] INFO   jTPCC : Term-00,18:28:56,388 \[Thread-127\] INFO   jTPCC : Term-00, Measured tpmC \(NewOrders\) = 101231.3318:28:56,388 \[Thread-127\] INFO   jTPCC : Term-00, Measured tpmTOTAL = 224908.1418:28:56,388 \[Thread-127\] INFO   jTPCC : Term-00, Session Start     = 2020-02-24 18:23:5618:28:56,388 \[Thread-127\] INFO   jTPCC : Term-00, Session End       = 2020-02-24 18:28:5618:28:56,388 \[Thread-127\] INFO   jTPCC : Term-00, Transaction Count = 1125698

