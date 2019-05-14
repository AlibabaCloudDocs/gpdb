# 如何查看本地客户端IP {#concept_gq5_qpk_2gb .concept}

## 问题描述 {#section_qx4_55w_pfb .section}

由于网络环境复杂多样，用户可能无法正确地找到本地客户端的IP地址来设置实例白名单。本文介绍如何查看本地客户端的IP。

下面以具体操作为例，描述详细的操作过程。

## 操作步骤 {#section_pyc_tzz_qfb .section}

1.  将`0.0.0.0/0`添加到AnalyticDB for PostgreSQL实例的白名单，具体操作如下：
    1.  登录[云数据库AnalyticDB for PostgreSQL管理控制台](https://gpdb.console.aliyun.com)。
    2.  选择目标实例所在地域。
    3.  单击目标实例的 ID， 进入实例**基本信息**页面。
    4.  在实例菜单栏中，选择**数据安全性**，进入数据安全性页面。

    5.  在白名单设置标签页中，单击 default 白名单分组后的**修改**，进入修改白名单分组页面。
    6.  删除**组内白名单**中的默认白名单 127.0.0.1，写入白名单地址`0.0.0.0/0`。

        **说明：** `0.0.0.0/0` 允许任何IP访问数据库，将会引入较高的安全风险，请尽快删除。

    7.  单击**确定**，完成白名单设置。
2.  使用客户端连接到AnalyticDB for PostgreSQL实例，参见[连接数据库](../../../../intl.zh-CN/快速入门/连接数据库.md#)下载安装psql客户端，使用如下连接语句连接数据库：

    ```
    psql -h yourgpdbaddress.gpdb.rds.aliyuncs.com -p 3432 -d postgres -U gpdbaccount
    ```

    其中，各个参数的定义如下：

    -   -h：指定主机地址。
    -   -p：指定端口号。
    -   -d：指定数据库（默认的数据库是 postgres），
    -   -U：指定连接的用户。
    -   可以通过`psql --help`查看更多选项。在 psql 中，可以执行`\?`查看更多 psql 中支持的命令。
3.  进入数据库后，在数据库的SQL命令行窗口中运行如下命令，查询客户端的IP地址。

    ```
    select * from pg_stat_activity;
    ```

    查询结果的CLIENT\_ADDR字段即为客户端的IP地址。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/80825/155781922938978_zh-CN.png)

4.  在AnalyticDB for PostgreSQL控制台中，将白名单`0.0.0.0/0`删除，输入上个步骤查询到的IP地址，即可正常访问数据库。

