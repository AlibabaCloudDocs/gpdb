# Quick BI连接

本文介绍如何通过阿里云Quick BI连接云原生数据仓库PostgreSQL版。

## 操作步骤

1.  登录 [Quick BI 控制台](http://das.base.shuju.aliyun.com/console.htm)。
2.  单击上方菜单栏中的**工作空间**。
3.  在工作空间页面单击左侧**数据源**。
4.  单击**新建数据源** \> **AnalyticDB for PostgreSQL**。
5.  在添加AnalyticDB for PostgreSQL数据源页面进行参数配置。

    **说明：**

    -   请在云原生数据仓库PostgreSQL版白名单中添加如下IP地址，Quick BI才能访问云原生数据仓库PostgreSQL版：10.152.69.0/24,10.152.163.0/24,139.224.4.0/24。
    -   关于如何设置白名单，请参见[设置白名单](/cn.zh-CN/快速入门/设置白名单.md)。
    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7393992951/p51309.png)

    |配置项|说明|
    |---|--|
    |**显示名称**|数据源名称。|
    |**数据库地址**|云原生数据仓库PostgreSQL版的连接地址，详情请参见[连接地址](/cn.zh-CN/实例管理/网络连接/管理外网地址.md)。|
    |**端口**|连接地址对应的端口号。|
    |**数据库**|云原生数据仓库PostgreSQL版数据库名。|
    |**Schema**|数据库Schema名。|
    |**用户名**|云原生数据仓库PostgreSQL版数据库账号。|
    |**密码**|云原生数据仓库PostgreSQL版数据库密码。|

6.  完成上述参数配置后，单击**连接测试**测试连通性，测试通过后，单击**添加**添加数据源。

    **说明：** 如果连通失败，请检查各配置项是否填写正确，确认无误后再进行连接测试。


## 使用 Quick BI

成功连接云原生数据仓库PostgreSQL版数据源后，您可以参见以下步骤在Quick BI中完成报表分析等操作。

-   [创建数据集]()
-   [创建仪表板]()
-   [创建电子表格]()
-   [新建数据门户]()

