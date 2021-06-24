# 自建Greenplum迁移到AnalyticDB PostgreSQL版

AnalyticDB PostgreSQL 6.0版基于Greenplum 6.0构建，并深度优化演进，支持向量化计算，在Multi-Master架构下支持事务处理，对外接口完全兼容社区版Greenplum。整体迁移分为应用迁移和数据迁移，应用层可以实现平滑迁移，数据迁移提供了多种方案。

自建Greenplum迁移到AnalyticDB PostgreSQL版整个迁移流程如下：

1.  评估迁移风险，指定迁移方案和计划。
2.  采购AnalyticDB PostgreSQL版测试实例，验证迁移方案（包含数据迁移验证和应用迁移验证）。
3.  采购AnalyticDB PostgreSQL版生产实例，迁移自建Greenplum生产库。
4.  业务连接到AnalyticDB PostgreSQL版生产实例，验证业务。

自建Greenplum的网络环境（AnalyticDB PostgreSQL版实例节点能否访问自建Greenplum节点）和版本（4X、5X、6X）的不同，则数据迁移方案也会有所不同，数据迁移主体步骤分为如下三步：

1.  DDL Schema迁移。
    -   library：pg\_dump不能直接导出Schema，需要您手动在AnalyticDB PostgreSQL版实例中创建Schema。
    -   插件（extension）：您需要确认自建Greenplum与AnalyticDB PostgreSQL版支持的插件差异，AnalyticDB PostgreSQL版支持的插件，请参见[扩展插件列表](/cn.zh-CN/开发进阶/高级扩展插件使用/扩展插件列表.md)。
    -   语法兼容：AnalyticDB PostgreSQL版内核版本为PG 9.4，与Greenplum 4X版本的语法存在部分语法不兼容的情况，需要您手动修改。
2.  表数据迁移。
    -   全量迁移和增量迁移：建议您通过业务层面来设计迁移，例如对表增加字段（时间戳或TAG）来区别增量数据。
    -   分区表或INHERIT表：建议从子表粒度迁移。
3.  数据校验与业务验证。
    -   关于数据校验，除了表COUNT总数校验外，SUM、MIN、MAX等计算字段也需要验证。
    -   关注视图、存储过程、业务SQL运行的正确性。

## AnalyticDB PostgreSQL版实例节点无法访问自建Greenplum节点

1.  **Schema迁移**
    1.  在自建Greenplum实例的Master节点，通过pg\_dumpall或pg\_dump工具导出DDL Schema，BASH命令示例如下：

        ```
        export PGHOST=<源实例host>
        export PGPORT=<源实例port>
        export PGUSER=<源实例超级用户>
        export PGPASSWORD=<源实例密码>
        
        #-- 4X实例全部schema导出
        pg_dumpall -s -q --gp-syntax  > full_schema.sql
        
        #-- 5X/6X实例全部schema导出
        pg_dumpall -s --gp-syntax  > full_schema.sql
        ```

    2.  使用psql登录自建Greenplum数据库，检查是否有自定义library，如果存在则需要手动在AnalyticDB PostgreSQL版数据库中创建library，检查语句如下：

        ```
        select * from pg_catalog.pg_library;
        ```

    3.  使用psql登录AnalyticDB PostgreSQL版实例，执行DDL Schema，bash命令示例如下：

        ```
        export PGHOST=<目标实例host>
        export PGPORT=<目标实例port>
        export PGUSER=<目标实例超级用户>
        export PGPASSWORD=<目标实例密码>
        
        psql postgres -f full_schema.sql > psql.log 2>&1 &
        ```

        检查psql.log是否有产生错误信息，并对错误信息进行分析解决。错误信息基本都与SQL语法有关（尤其4X版本），不兼容项的相关检查，请参见[4.3版本与6.0版本兼容性注意事项](/cn.zh-CN/实例管理/版本管理/4.3版本与6.0版本兼容性注意事项.md)和[4.3版升级6.0版不兼容项检查参考指南](/cn.zh-CN/实例管理/版本管理/4.3版升级6.0版不兼容项检查参考指南.md)。

2.  **表数据迁移**
    1.  导出自建Greenplum的表数据，您可以通过COPY TO或GPFDIST外表两种方法导出：
        -   COPY TO

            在psql客户端使用COPY TO命令导出表数据，SQL示例如下：

            ```
            -- 4X/5x/6x
            COPY public.t1 TO '/data/gpload/public_t1.csv' FORMAT CSV ENCODING 'UTF8';
            
            -- 5x/6x支持 on segment导出
            -- 这里的<SEGID>不要改，系统内部会识别，<SEG_DATA_DIR>可以自定义
            COPY public.t1 TO '<SEG_DATA_DIR>/public_t1_<SEGID>.csv' FORMAT CSV ENCODING 'UTF8' 
            ON SEGMENT;
            ```

            COPY命令具体使用方式，请参见[COPY命令导入或导出本地数据](/cn.zh-CN/数据接入/COPY命令导入或导出本地数据.md)。

        -   GPFDIST外表

            通过GPFDIST外表导出数据，您需要先启动GPFDIST服务（GPFDIST在自建Greenplum启动程序的bin目录下），bash命令参见：

            ```
            mkdir -p /data/gpload
            gpfdist -d /data/gpload -p 8000 & 
            ```

            使用psql连接自建Greenplum数据库，导出表数据，SQL示例如下：

            ```
            -- public.t1为已知要迁移的表
            CREATE WRITABLE EXTERNAL TABLE ext_w_t1 (LIKE public.t1)
            LOCATION ('gpfdist://<host>:8000/public_t1.csv') FORMAT 'CSV' ENCODING 'UTF8';
            
            -- 写入外表
            insert into ext_w_t1 select * from public.t1;
            ```

    2.  将导出的数据上传到阿里云OSS上，关于阿里云OSS的详细信息，请参见[什么是对象存储OSS](/cn.zh-CN/产品简介/什么是对象存储OSS.md)。

        **说明：** 阿里云OSS需要和AnalyticDB PostgreSQL版实例在同一地域。

    3.  使用psql客户端连接AnalyticDB PostgreSQL版实例，创建OSS Foreign Table，通过读取OSS Foreign Table将数据写入目标表，具体操作方式，请参见[使用 OSS Foreign Table 访问 OSS 数据](/cn.zh-CN/开发入门/使用 OSS Foreign Table 访问 OSS 数据.md)。
3.  **数据验证**

    对比AnalyticDB PostgreSQL版实例内的数据与自建Greenplum内的数据，确保数据和业务没有区别。

    数据验证分为数据一致性验证和业务一致性验证：

    -   数据一致性验证：您可以全量对比数据也可以抽样对比数据，抽样对比需要重点验证表数据量是否一致和数值类型字段SUM、AVG、MIN、MAX是否一致两点。
    -   业务一致性验证：您需要将应用系统配置源指向AnalyticDB PostgreSQL版实例，检查业务功能正常性和执行结果正确性。

## AnalyticDB PostgreSQL版实例节点可以访问自建Greenplum节点

基于GPTRANSFER工具来迁移DDL Schema和数据。

GPTRANSFER是4X和5X版本支持的数据迁移工具，由于4X版本和AnalyticDB PostgreSQL版存在部分语法差异，建议您将自建Greenplum升级至5X版本再通过GPTRANSFER工具迁移。

GPTRANSFER工具可从Greenplum源码的5X\_STABLE分支的/gpMgmt/bin路径获取，编译部署后在bin目录下。

**说明：** 全量数据迁移过程中，建议不要往自建Greenplum实例中写入数据。

**GPTRANSFER命令**

将GPTRANSFER中几处代码注销后方可正常执行，需要注销的代码如下：

```
cmd = MakeDirectory('Create work directory',
                    self._work_dir, REMOTE, self._options.dest_host)
self._pool.addCommand(cmd)
```

```
cmd = RemoveDirectory('Remove work directory',
                      self._work_dir, REMOTE, self._options.dest_host)
self._pool.addCommand(cmd)
```

在自建Greenplum实例的master节点执行GPTRANSFER命令，全量数据迁移命令示例请参见：

```
-- 拉起源实例的linux当前用户密码
export SSHPASS=......

python gptransfer -a --validate count --batch-size 8 --max-line-length 104857600 \
--source-host <源实例host> --source-port <源实例port> --source-user <源实例linux当前用户> \
--dest-host <ADB PG数据库链接串> --dest-port <ADB PG数据库端口> --dest-user <ADB PG数据库用户> \
--source-map-file <源实例segment ip配置文件> --work-base-dir <工作目录> \
--full --truncate --analyze --no-final-count > gptransfer.log &
```

其中参数介绍如下：

-   `--batch-size`：并行度，可以并行传输多少张表，默认值为2，最大值为10。
-   `--source-map-file`：源实例所有SEGMENT IP地址保存的文件（行内容唯一），格式如下：

    ```
    seg1_ip,seg1_ip
    seg2_ip,seg2_ip
    ```


GPTRANSFER也支持指定单库单表迁移，详细可执行`python gptransfer --help`命令查看帮助。

**GPTRANSFER注意项**

-   GPTRANSFER不支持增量迁移。
-   自建Greenplum用到的Extension包含在AnalyticDB PostgreSQL版实例支持的Extension内。
-   自建Greenplum如果重度使用索引，建议先删除索引，等数据迁移完再重建，将有利于加速数据迁移。
-   自建Greenplum环境所有节点的8000～9999端口需要开放。
-   AnalyticDB PostgreSQL版实例能够访问自建Greenplum环境的gpfdist服务（可在AnalyticDB PostgreSQL版创建可读外表，测试SELECT是否返回结果）。
-   如果gpfdist服务只能通过外网访问（注意外网流量是否限制），需要修改gptransfer部分逻辑方可执行。
-   在自建Greenplum实例master节点，需要psql能够免密连接到AnalyticDB PostgreSQL版实例（AnalyticDB PostgreSQL版实例控制台需要设置自建Greenplum实例master节点IP白名单）。

如果gpfdist服务只能通过外网访问，修改gptransfer部分逻辑地方请参见下图。

![GPTRANSFER工具](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6378254261/p287128.png)

-   修改`urls`参数初始化值，将内网地址改为外网地址。
-   检查gptransfer.log是否有错误信息输出，分析错误信息并解决。

## 其他迁移方式

您可也以使用阿里云DataWorks工具来迁移数据。

-   DataWorks简介，请参见[DataWorks官网](https://www.aliyun.com/product/bigdata/ide)。
-   DataWorks集成指导，请参见[Dataworks数据集成](/cn.zh-CN/数据接入/Dataworks数据集成.md)。
-   DataWorks调度配置，请参见[Dataworks作业调度](/cn.zh-CN/数仓开发/Dataworks作业调度.md)。

