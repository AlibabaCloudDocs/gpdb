# TPC-B

AnalyticDB for PostgreSQL 6.0 \(ADBPG6.0\) has made a qualitative leap in terms of transaction processing performance compared with the previous ADBPG4.3, this article demonstrates the transaction processing capability of the ADBPG6.0 by using TPC-B benchmark of the industry-standard transaction performance test.

## TPC-B overview

TPC-B is a benchmark provided by the Transaction Processing Performance Council \(TPC\). It mainly measures the number of concurrent transactions a system can process per second. TPC-B does not simulate a specific transaction scenario in real life like TPC-C. Transactions in this scenario are transactions without semantics, which are composed of simple SQL statements \(the transaction is mixed with the insertion of large tables and small tables, update and query operations\). In addition, each client request will not have a TPC-C human think time like the gap. Instead, once the previous transaction is completed, the next transaction request will be sent immediately. Therefore, TPC-B is often used to stress test the transaction performance of database systems. The TPC-B performance metric is the number of Transactions processed per Second, that is, TPS\(Transactions per Second\).

## Test environment

-   Test data

Use the open-source tool pgbench in PostgreSQL to generate TPC-B test data. The filling factor is 100 and the scale factor is 11424. The data volume of each table in the last TPC-B is as follows:

|Table name|Data volume \(number of rows\)|
|----------|------------------------------|
|pgbench\_accounts|1142400000|
|pgbench\_branches|11424|
|pgbench\_history|0|
|pgbench\_tellers|114240|

-   Test cluster

The ADBPG6.0 cluster to be tested consists of 16 compute nodes. Each Compute Node has 4 CPU cores and 32GB of memory. The storage type is high-performance SSD. For more information about specifications, see: [Instance specifications](https://www.alibabacloud.com/help/doc-detail/35406.htm). In addition, an ECS instance is created in the same domain and located in the same VPC as the ADBPG instance. You can run the pgbench command to generate a TPC-B load and send a request to the ADBPG instance.

-   Cluster configuration parameter

To obtain the ultimate TP performance, the following parameters need to be modified. Some of the parameters cannot be set by the user. Contact the ADBPG personnel on duty to modify them.

|Parameter|Description|
|---------|-----------|
|set optimizer = off|Turning off the maid optimizer for AP scenarios is more friendly to TP performance.|
|set shared\_buffers = 8GB|Increase the data sharing cache of the ADBPG instance. Modifying this parameter requires restarting the instance.|
|set wal\_buffers = 256MB|Increase the WAL log cache of the ADBPG. Modifying this parameter requires restarting the instance.|
|set log\_statement = none|Disables log output.|
|set random\_page\_cost = 10|Reducing the cost of random access is beneficial to queries.|
|set gp\_resqueue\_priority = off set resource\_scheduler = off 

|Closes the resource queue of the ADBPG instance. Instance restart required|

## Test result

This test shows the performance of ADBPG6.0 with different concurrency levels. The results are shown in the following figure, where x-axis indicates different parallelism and y-axis indicates the test performance \(in TPS or transactions per second\). In addition to the performance test on the TPC-B of ADBPG, pgbench is also used to test read-only, update-only, and insert-only. As shown in the following graph of the test performance result, the peak TPC-B performance of ADBPG can reach 5923 TPS, the peak read-only performance is 150084 TPS, and the peak update performance is 31023 TPS. The insert-only peak performance is 60367 TPS.

-   TPC-B performance

|Concurrency|1|8|16|32|48|64|96|128|168|192|
|-----------|--|--|--|--|--|--|--|---|---|---|
|TPS|199|1481|2031|2568|2795|3236|3622|5923|5037|5211|
|RT\(ms\)|5.02|5.4|7.88|12.5|17.2|19.8|26.5|21.6|33.3|36.8|

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7743560951/p111571.png)

-   Read-only performance

|Concurrency|1|8|16|32|48|64|96|128|160|192|
|-----------|--|--|--|--|--|--|--|---|---|---|
|TPS|1,875|14226|26784|51115|72767|92370|125708|143297|150084|139637|
|RT\(ms\)|0.53|0.56|0.6|0.63|0.66|0.69|0.76|0.89|1.07|1.37|

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7743560951/p111576.png)

-   Update-only performance

|Concurrency|1|8|16|32|48|64|96|128|160|192|
|-----------|--|--|--|--|--|--|--|---|---|---|
|TPS|787|6486|12127|19333|21083|24199|31023|27464|24362|23600|
|RT\(ms\)|1.27|1.23|1.32|1.66|2.28|2.64|3.09|4.66|6.57|8.14|

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7743560951/p111578.png)

-   Insert-only performance

|Concurrency|1|8|16|32|48|64|96|128|160|192|
|-----------|--|--|--|--|--|--|--|---|---|---|
|TPS|1497|11463|21771|40543|56887|59967|60367|59365|53375|48927|
|RT\(ms\)|0.69|0.7|0.73|0.79|0.84|1.07|1.59|2.16|3.0|3.9|

![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/7743560951/p111579.png)

## How to use pgbench to test the TPC-B of the ADBPG application

-   Tool download and installation

There are two methods to install pgbench:

1.  Source code installation: download the source code of the open-source database PostgreSQL, and compile pgbench into an executable binary file in the corresponding directory of pgbench.
2.  Binary installation: you can first yum install postgresql-server to install the PostgreSQL program. During this process, the pgbench tool is automatically installed. ,,,

-   Data Loading

Run the following command to automatically load the generated data to the ADBPG instance.

```
# Parameter-F is the filling factor mentioned above, and parameter-s is worth the scaling factor
./pgbench -i -F 100 -s 11424 -p port -h con_addr -U user_name db_name
```

-   Perform tests

Execution TPC-B load

```
# We recommend that you set-c to the same number of clients and-j to the same number of threads for establishing connections.
# -T: specifies the test execution time, in seconds.
./pgbench -h con_addr -p port -r -n -c 96 -j 96 -T 120 -f all.sql -U user_name db_name
```

The all.sql statement contains the TPC-B payload. Its content is as follows:

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

-   Execute read-only load

```
# We recommend that you set-c to the same number of clients and-j to the same number of threads for establishing connections.
# -T: specifies the test execution time, in seconds.
./pgbench -h con_addr -p port -r -n -c 96 -j 96 -T 120 -f select.sql -U user_name db_name
```

The select.sql statement is read-only. Its content is as follows:

```
\set scale 11424
\set naccounts 100000 * :scale
\setrandom aid 1 :naccounts

SELECT abalance FROM pgbench_accounts WHERE aid = :aid;
				
```

-   Perform a load-only update

```
# We recommend that you set-c to the same number of clients and-j to the same number of threads for establishing connections.
# -T: specifies the test execution time, in seconds.
./pgbench -h con_addr -p port -r -n -c 96 -j 96 -T 120 -f update.sql -U user_name db_name
```

The update.sql file is used to update only the payload.

```
\set scale 11424
\set naccounts 100000 * :scale
\setrandom aid 1 :naccounts
\setrandom delta -5000 5000
UPDATE pgbench_accounts SET abalance = abalance + :delta WHERE aid = :aid;
```

-   Execute insert load only

```
# We recommend that you set-c to the same number of clients and-j to the same number of threads for establishing connections.
# -T: specifies the test execution time, in seconds.
./pgbench -h con_addr -p port -r -n -c 96 -j 96 -T 120 -f insert.sql -U user_name db_name
```

insert.sql is an insert-only payload. Its content is as follows:

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

