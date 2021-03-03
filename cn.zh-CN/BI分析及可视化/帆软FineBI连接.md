# 帆软FineBI连接

分析型数据库PostgreSQL版基于开源数据库Greenplum构建，兼容Greenplum和PostgreSQL的语法、接口和生态。本文介绍如何通过FineBI连接分析型数据库PostgreSQL版。

## 前提条件

-   下载并安装FineBI。
-   已创建分析型数据库PostgreSQL版，详情请参见[创建实例](/cn.zh-CN/快速入门/创建实例.md)。
-   已在分析型数据库PostgreSQL版白名单中添加FineBI所在服务器IP地址，添加白名单请参见[设置白名单](/cn.zh-CN/快速入门/设置白名单.md)。

## 注意事项

首次连接PostgreSQL数据源，需要下载JDBC Driver，下载地址和安装方式请参见[FineBI文档](https://help.finereport.com/doc-view-2562.html)。

## 操作步骤

1.  启动FineBI客户端。
2.  在左侧导航栏选择**系统管理** \> **数据连接** \> **数据连接管理**。
3.  在数据连接管理页面单击**新建数据连接**，选择**Pivotal Greenplum Database**。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/3905519751/p51295.png)

4.  在数据连接管理页面填写**URL**、**用户名**和**密码**等信息。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4905519751/p51474.png)

    |配置项|描述|示例|
    |---|--|--|
    |驱动器|驱动器代码。|`org.postgresql.Driver`|
    |URL|数据源连接信息。不同的驱动器，对应的URL格式不同，URL格式请参见[FineBI驱动器配置信息](#table_j2l_1kj_d1k)。 **说明：**

    -   连接地址可以在分析型数据库PostgreSQL版控制台基本信息页面进行查看。
    -   若FineBI与数据源不在同一可用区，需要通过外网地址访问，分析型数据库PostgreSQL版申请外网地址请参见[管理外网地址](/cn.zh-CN/实例管理/网络连接/管理外网地址.md)。
    -   分析型数据库PostgreSQL版默认端口为3432。
|`jdbc:postgresql://gp-bpxxxxxxxxxxxxxx.gpdb.rds.aliyuncs.com:3432/dbname`|
    |编码|默认为自动。|-|
    |用户名|分析型数据库PostgreSQL版数据库账号。|`adbpgname`|
    |密码|分析型数据库PostgreSQL版数据库密码。|`adbpgpassword`|

    |驱动器代码|URL|支持数据库版本|
    |-----|---|-------|
    |`org.postgresql.Driver`|`jdbc:postgresql://连接地址:端口/数据库名称`|4.3.9；5.0|
    |`com.pivotal.jdbc.GreenplumDriver`|`jdbc:pivotal:greenplum://连接地址:端口;DatabaseName=数据库名称`|

5.  连接信息填写完成后单击**测试连接**。

    **说明：** 如果测试连接失败，可以根据错误项的提示进行修改。

6.  测试连接通过后，点击右上角**保存**即可。

