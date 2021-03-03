# BI工具兼容概述

AnalyticDB for PostgreSQL 基于开源数据库 Greenplum 构建，兼容 Greenplum 接口及相关工具，兼容业界主流 BI 工具，也兼容阿里云提供的QucikBI 及 DataV 等数据智能和展现工具。针对业界主流工具，用户可以选择以 Greenplum 或 PostgreSQL 作为数据源类型来连接实例。 JDBC Driver 可以考虑BI工具自带的 Driver，或采用[连接数据库](/intl.zh-CN/快速入门/客户端连接.md) 推荐的 JDBC Driver。

BI工具兼容支持列表

|BI 工具|使用指导|
|-----|----|
|阿里云 QuickBI|请参见[Quick BI连接](/intl.zh-CN/BI分析及可视化/Quick BI连接.md)，选择AnalyticDB for PostgreSQL（或HybridDB for PostgreSQL）作为数据源。|
|阿里云 DataV|请参见[DataV](https://data.aliyun.com/visual/datav)选择AnalyticDB for PostgreSQL（或HybridDB/HybridDB for PostgreSQL）作为数据源。|
|Tableau|请参见[Tableau连接](/intl.zh-CN/BI分析及可视化/Tableau连接.md),选择Greenplum类型数据源连接实例。|

