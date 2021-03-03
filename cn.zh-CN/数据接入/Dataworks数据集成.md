# Dataworks数据集成

[数据集成（Data Integration）](https://www.aliyun.com/product/cdp/)是阿里巴巴集团提供的数据同步平台。该平台具备可跨异构数据存储系统、可靠、安全、低成本、可弹性扩展等特点，可为20多种数据源提供不同网络环境下的离线（全量/增量）数据进出通道。详情请参见[支持的数据源与读写插件]()。

## 应用场景

-   AnalyticDB for PostgreSQL 可以通过数据集成的同步任务将数据同步到到其他的数据源中（AnalyticDB for PostgreSQL数据导出），并对数据进行相应的处理。
-   可以通过数据集成的同步任务将处理好的其他数据源数据同步到 AnalyticDB for PostgreSQL（AnalyticDB for PostgreSQL数据导入）。

无论是哪种应用场景，都可以通过DataWorks的数据集成功能完成数据的同步过程，详细的操作步骤（包括创建数据集成任务、数据源配置、作业配置、白名单配置等），请参考[DataWorks文档](https://help.aliyun.com/product/72772.html?spm=a2c4g.11186623.3.1.12c15d4ctNn3vx)中的使用指南--\>数据集成一栏。文章中余下部分会介绍AnalyticDB for PostgreSQL的数据导入导出操作详细步骤。

## 准备工作

数据集成任务准备

1.  开通[准备阿里云账号]()
2.  开通MaxCompute，自动产生一个默认的MaxCompute数据源，并使用主账号登录[DataWorks](https://data.aliyun.com/product/ide?spm=a2c4g.11186623.2.18.2dc845d0LfFDRS)
3.  [创建工作空间]()。您可在工作空间中协作完成工作流，共同维护数据和任务等，因此使用DataWorks前需要先创建工作空间。

**说明：** 如果您想通过子账号创建数据集成任务，可以赋予其相应的权限。详情请参见[准备RAM用户]()

AnalyticDB for PostgreSQL 准备

1.  进行数据导入操作前，请通过 PostgreSQL 客户端创建好 AnalyticDB for PostgreSQL中需要迁入数据的目标数据库和表。
2.  对于数据导出，请登录AnalyticDB for PostgreSQL的管理控制台进行IP**白名单设置**，详情请参见 [添加白名单]()

    ![添加白名单](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6834032951/p63279.png)


## 数据导入

源端的数据源需要在DataWorks管理控制台进行添加，数据源添加的详细步骤请参考[配置AnalyticDB for PostgreSQL数据源]()

配置同步任务：

配置好数据源后，就可以配置同步任务，完成数据源数据到AnalyticDB for PostgreSQL的数据导入。配置同步任务有两种模式：**向导模式**和**脚本模式**。

-   向导模式。通过向导模式配置数据集成任务，需要依次完成以下几步：

    1.  新建数据同步节点；
    2.  选择数据来源；
    3.  选择数据去向（这里的数据去向一定是AnalyticDB for PostgreSQL）；
    4.  配置字段的映射关系；
    5.  配置作业速率上限、脏数据检查规则等信息；
    6.  配置调度属性。
    **说明：** 具体操作步骤请参考DataWorks[通过向导模式配置任务]()

-   脚本模式。通过脚本模式配置数据集成任务，需要依次完成以下几步：

    1.  新建数据同步节点；
    2.  导入模板；
    3.  配置同步任务的读取端；
    4.  配置同步任务的写入端（这里写入端一定是AnalyticDB for PostgreSQL）；
    5.  配置字段的映射关系；
    6.  配置作业速率上限、脏数据检查规则等信息；
    7.  配置调度属性。
    **说明：** 具体操作步骤请参考DataWorks[通过脚本模式配置任务]()


## 数据导出

数据导出的步骤和数据导入的步骤一样，区别是在数据导出中，数据源配置需要配置为AnalyticDB for PostgreSQL（参见[配置AnalyticDB for PostgreSQL数据源]()），而目的端可以配置为其他的数据源类型。

## 参考信息

更多数据集成详细信息请参考[DataWorks文档](https://help.aliyun.com/product/72772.html?spm=a2c4g.11186623.3.1.12c15d4ctNn3vx)

