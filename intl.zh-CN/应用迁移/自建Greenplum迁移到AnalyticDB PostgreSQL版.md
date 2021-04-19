# 自建Greenplum迁移到AnalyticDB PostgreSQL版

云原生数据仓库AnalyticDB PostgreSQL版完全兼容开源Greenplum，支持应用平滑迁移。本文主要描述如何从自建Greenplum迁移到AnalyticDB PostgreSQL版。

## 迁移方案

AnalyticDB PostgreSQL 6.0版基于开源Greenplum 6.0构建，并深度优化演进，支持向量化计算，在Multi-Master架构下支持事务处理，对外接口完全兼容社区Greenplum。整体迁移分为应用迁移和数据迁移。应用层迁移可以做到完全平滑实现，数据提供多种方案。

![gp2adbpg](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1204391951/p93164.png)

1.  在阿里云规划并创建实例，规格设计可以参考[规格及选型](/intl.zh-CN/规格和定价/规格及选型.md)。
2.  迁移表DDL，AnalyticDB PostgreSQL版完全兼容开源Greenplum，从自建GP可以基于pg\_dumpall来导出所有表定义的DDL语句，可以在AnalyticDB PostgreSQL版上直接建立对应的表结构。

    ```
    pg_dumpall --gp-syntax --schema-only > db_dump.sql
    ```

3.  数据迁移：数据迁移推荐如下三种方案，可以根据业务需求进行规划选择。

    -   通过Dataworks数据集成，不落地逐表进行数据全量迁移。
    -   从自建GP导出数据，并上传到阿里云ECS，通过AnalyticDB PostgreSQL版的COPY命令工具导入数据到AnalyticDB PostgreSQL版实例，具体操作请参见[COPY命令导入或导出本地数据](/intl.zh-CN/数据接入/COPY命令导入或导出本地数据.md)。
    -   从自建GP导出数据，并上传到阿里云对象存储，通过AnalyticDB PostgreSQL版的OSS外表并行导入功能，高速加载数据到AnalyticDB PostgreSQL版实例，具体操作请参见[OSS外表高速导入或导出OSS数据](/intl.zh-CN/数据接入/OSS外表高速导入或导出OSS数据.md)。
    dataworks数据同步操作简单，但相对数据同步和加载速度较慢。对于COPY和OSS两种加载方式，OSS外表并行加载要快于COPY命令的数据加载。


