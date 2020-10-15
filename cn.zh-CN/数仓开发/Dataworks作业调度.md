# Dataworks作业调度

使用Dataworks可以使云原生数据仓库PostgreSQL版获得任务开发、任务依赖关系管理、任务调度、任务运维等全方位强大的能力，进一步增强分析型数据库PostgreSQL版的ETL能力。本文将介绍如何使用DataWorks来调度云原生数据仓库PostgreSQL版的脚本任务。

## 数据准备

-   测试数据来源于[TPCH的测试数据集](http://www.tpc.org/tpch/)。
-   如何将数据导入云原生数据仓库PostgreSQL版请参见[数据迁移及同步方案综述](/cn.zh-CN/数据接入/数据迁移及同步方案综述.md)。

## 任务简介

任务之间的依赖是任务调度中的一个重要的功能，例如，我们在DataWorks里面创建两个云原生数据仓库PostgreSQL版的任务，其中表与任务之间的关系如下图：

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0603266951/p58666.png)

-   任务：finished\_orders

    从orders表清洗出已经完成的订单：`o_orderstatus = 'F'`，并写入 finished\_orders 表。

-   任务：high\_value\_finished\_orders

    从finished\_orders表里面找出总价大于10000的订单：`o_totalprice > 10000`，并写入 high\_value\_finished\_orders 表。


## 创建任务

如何创建任务请参见[创建AnalyticDB for PostgreSQL节点]()。

## 配置任务依赖

任务调度的核心在于多个任务在指定时间按照指定的依赖关系运行。

例如，任务`finished_orders` 在每天2点开始运行，任务`high_value_finished_orders`在任务`finished_orders`成功运行之后再运行。具体操作如下所示：

1.  设置任务`finished_orders`运行时间。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0603266951/p58718.png)

2.  配置任务`high_value_finished_orders` 任务依赖。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/0603266951/p58720.png)

    ​任务依赖如果无法自动解析，可以手动指定上游依赖节点。

    ![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1603266951/p58721.png)


## 任务发布

选择上述步骤提交的任务，点击发布按钮，如下图所示。

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1603266951/p58722.png)

您可以在发布列表页面可以查看任务是否发布成功。

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1603266951/p58725.png)

发布成功之后，您可以进入任务运维页面查看配置的任务，并进行各种运维操作。

![](https://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/zh-CN/1603266951/p58726.png)

