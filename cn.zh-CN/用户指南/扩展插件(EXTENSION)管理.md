# 扩展插件\(EXTENSION\)管理 {#concept_bnw_sty_52b .concept}

云数据库 AnalyticDB for PostgreSQL 基于 Greenplum Database 开源数据库项目开发，由阿里云深度扩展，是一种在线的分布式云数据库，由多个[计算组](../../../../cn.zh-CN/产品简介/名词解释.md#)组成，可提供大规模并行处理（MPP）数据仓库的服务。

## 插件类型 {#section_knh_yty_52b .section}

云数据库 AnalyticDB for PostgreSQL 支持如下插件\(EXTENSION\)：

-   PostGIS：支持地理信息数据。

-   MADlib：机器学习方面的函数库。

-   fuzzystrmatch：字符串模糊匹配。

-   orafunc：兼容 Oracle 的部分函数。

-   oss\_ext：支持从 OSS 读取数据。

-   hll：支持用 HyperLogLog 算法进行统计。

-   pljava：支持使用 PL/Java 语言编写用户自定义函数（UDF）。

-   pgcrypto：支持加密函数。

-   intarray：整数数组相关的函数、操作符和索引支持。


## 创建插件 {#section_nnh_yty_52b .section}

创建插件的方法如下所示：

```
CREATE EXTENSION <extension name>;
CREATE SCHEMA <schema name>;
CREATE EXTENSION IF NOT EXISTS <extension name> WITH SCHEMA <schema name>;
```

**说明：** 创建 MADlib 插件时，需要先创建 plpythonu 插件，如下所示：

```
CREATE EXTENSION plpythonu;
CREATE EXTENSION madlib;
```

## 删除插件 {#section_snh_yty_52b .section}

删除插件的方法如下所示：

**说明：** 如果插件被其它对象依赖，需要加入 CASCADE（级联）关键字，删除所有依赖对象。

```
DROP EXTENSION <extension name>;
DROP EXTENSION IF EXISTS <extension name> CASCADE;
```

