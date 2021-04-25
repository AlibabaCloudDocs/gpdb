# 通过外表访问HDFS数据

注：该功能公共云预计6月份上线

AnalyticDB for PostgreSQL 支持从Hadoop集群中读取数据，并写入数据到Hadoop集群中，用到的工具有外部表 external tables 和 gphdfs 协议。本文主要介绍在AnalyticDB for PostgreSQL中使用gphdfs协议向HDFS读写数据的步骤.

本文内容包括：

-   创建HDFS测试文件
-   创建HDFS读外表并查询数据
-   创建HDFS写外表并写入数据
-   通过外表访问Hive
-   通过外表访问HBase

## 创建HDFS测试文件

登录到HDFS，创建相应测试目录和文件，操作如下：

```
root@namenode:/# hadoop fs -mkdir /test #在根目录下创建一个test文件夹
root@namenode:/# echo "1 abc" > data.txt #创建一个测试数据文件
root@namenode:/# hadoop fs -put local.txt /test #上传测试数据文件到hdfs中
root@namenode:/# hadoop fs -cat /test/data.txt #查看文件内容
1 abc
```

## 创建HDFS读外表并查询数据

在外表创建语句中，指定HDFS集群的地址（使用gphdfs外表访问协议），以及关联的文件路径，文件格式和分隔符。

```
postgres=# CREATE READABLE EXTERNAL TABLE test (id int, name text)
LOCATION ('gphdfs://namenode_IP:port/test/data.txt')
FORMAT 'text' (delimiter ' ');
```

从外部表中读取数据：

```
postgres=# select * from test;
 id | name
----+------
  1 | abc
(1 row)
```

## 创建HDFS写外表并写入数据

在创建外部表的语句中，声明writtable，表示可写外部表：

```
postgres=# create WRITABLE external table test_write (id int, name text)
location('gphdfs://namenode_IP:port/test/data.txt')
FORMAT 'text' (delimiter ' ');
```

使用insert语句写入数据：

```
postgres=# insert into test_write values(2, 'def');
```

在HDFS集群上查看文件, 确认数据已正确写入：

```
root@namenode:/# hadoop fs -cat /test/data.txt #查看文件内容
1 abc
2 def
```

