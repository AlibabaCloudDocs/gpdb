# 阿里云Quick BI连接分析型数据库PostgreSQL版 {#concept_1113385 .concept}

本文介绍如何通过阿里云Quick BI连接分析型数据库PostgreSQL版。

## 操作步骤 {#section_hjd_tl7_3g3 .section}

1.  登录 [Quick BI 控制台](http://das.base.shuju.aliyun.com/console.htm)。
2.  单击上方菜单栏中的**工作空间**。
3.  在工作空间页面单击左侧**数据源**。
4.  单击**新建数据源** \> **AnalyticDB for PostgreSQL**。
5.  在添加AnalyticDB for PostgreSQL数据源页面进行参数配置。

    **说明：** 

    -   请在分析型数据库PostgreSQL版白名单中添加如下IP地址，Quick BI才能访问分析型数据库PostgreSQL版：10.152.69.0/24,10.152.163.0/24,139.224.4.0/24。
    -   关于如何设置白名单，请参见[设置白名单](cn.zh-CN/用户指南/管理实例/设置白名单.md#)。
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/895693/156531457251309_zh-CN.png)

    |配置项|说明|
    |---|--|
    |**显示名称**|数据源名称。|
    |**数据库地址**|分析型数据库PostgreSQL版的连接地址，详情请参见[连接地址](cn.zh-CN/用户指南/管理实例/申请外网地址.md#)。|
    |**端口**|连接地址对应的端口号。|
    |**数据库**|分析型数据库PostgreSQL版数据库名。|
    |**Schema**|数据库Schema名。|
    |**用户名**|分析型数据库PostgreSQL版数据库账号。|
    |**密码**|分析型数据库PostgreSQL版数据库密码。|

6.  完成上述参数配置后，单击**连接测试**测试连通性，测试通过后，单击**添加**添加数据源。

    **说明：** 如果连通失败，请检查各配置项是否填写正确，确认无误后再进行连接测试。


## 使用 Quick BI {#section_hgx_wvw_cnw .section}

成功连接分析型数据库PostgreSQL版数据源后，您可以参见以下步骤在Quick BI中完成报表分析等操作。

-   [创建数据集](https://help.aliyun.com/document_detail/86332.html)
-   [制作仪表板](https://help.aliyun.com/document_detail/54785.html)
-   [制作电子表格](https://help.aliyun.com/document_detail/100675.html)
-   [制作数据门户](https://help.aliyun.com/document_detail/48567.html)

