# BI工具兼容概述 {#concept_844289 .concept}

AnalyticDB for PostgreSQL 基于开源数据库 Greenplum 构建，兼容 Greenplum 接口及相关工具，兼容业界主流 BI 工具，也兼容阿里云提供的QucikBI 及 DataV 等数据智能和展现工具。针对业界主流工具，用户可以选择以 Greenplum 或 PostgreSQL 作为数据源类型来连接实例。 JDBC Driver 可以考虑BI工具自带的 Driver，或采用 [连接数据库](../../../../cn.zh-CN/快速入门/连接数据库.md#)推荐的 JDBC Driver。

BI工具兼容支持列表

|BI 工具|使用指导|
|-----|----|
|阿里云 QuickBI|请参见[QuickBI](https://data.aliyun.com/product/bi)，选择AnalyticDB for PG（或HybridDB for PG）作为数据源|
|阿里云 DataV|请参见[DataV](https://data.aliyun.com/visual/datav)选择AnalyticDB for PG（或HybridDB/HybridDB for PG）作为数据源|
|Tableau|请参加 [https://yq.aliyun.com/articles/706362](Tableau%20BI%E5%B7%A5%E5%85%B7%E5%AF%B9%E6%8E%A5%20AnalyticDB%20for%20PostgreSQL%E6%95%B0%E6%8D%AE%E6%BA%90),选择Greenplum类型数据源连接实例|
|帆软|请参见[帆软BI对接AnalyticDB for PG数据源](https://yq.aliyun.com/articles/706336),选择Greenplum类型数据源连接实例|

