# 开启SQL审计 {#concept_ezs_5gf_hfb .concept}

HybridDB for PostgreSQL控制台提供SQL审计功能，您可通过该功能查看SQL明细、定期审计SQL。开通SQL审计功能后，实例性能不会受到影响。

## 背景信息 { .section}

SQL审计会统计所有DML和DDL操作信息，这些信息是系统通过网络协议分析所得，在SQL查询量较大的时候可能会丢失少量记录。

## 注意事项 { .section}

-   SQL审计记录的保存时间为30天。
-   实例默认关闭SQL审计功能。开启该功能后，实例会产生额外费用，详细收费标准请参见[云数据库HybridDB for PostgreSQL 详细价格信息](https://www.aliyun.com/price/product?#/gpdb/detail)。

## 操作步骤 { .section}

1.  登录[云数据库HybridDB for PostgreSQL管理控制台](https://gpdb.console.aliyun.com)。
2.  在实例列表上方选择实例所在**地域**。
3.  找到目标实例，在操作栏中，单击**管理**。
4.  单击左侧导航的**数据安全性**。
5.  选择SQL审计页签，单击**开启SQL审计**。
6.  在弹出的对话框中单击**确定**，开启SQL审计功能。
    -   在开始SQL审计功能后，您可通过**时间**、**DB**、**User**、**关键字**等条件查询SQL信息。
    -   您还可在SQL审计页签，通过**关闭SQL审计**按键关闭已经开通的SQL审计。

