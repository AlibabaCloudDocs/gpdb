# 使用DTS将RDS PostgreSQL实时同步数据至AnalyticDB for PostgreSQL {#concept_845282 .concept}

[数据传输服务](https://dts.console.aliyun.com/#/home/)（Data Transmission Service，简称DTS）支持将RDS PostgreSQL数据同步至AnalyticDB for PostgreSQL。通过DTS提供的数据同步功能，可以轻松实现数据的流转，将企业数据集中分析。

## 前提条件 {#section_d8q_1qa_ssm .section}

-   源RDS PostgreSQL和目标AnalyticDB for PostgreSQL的数据表必须建有主键，该唯一标识用于在同步任务中断重做时，保证记录唯一性。
-   源库需为RDS PostgreSQL 9.4及以上版本或10.0 及以上版本。

## 注意事项 {#section_z1x_9n3_lft .section}

-   一个数据同步任务只能对一个数据库进行数据同步，如果有多个数据库需要同步，则需要为每个数据库创建数据同步任务。
-   对于已经启动同步任务的源库，若源库中创建新表并要将其加入同步任务，要求对该表执行如下操作，从而保证该表数据同步的一致性 ：（注：对于首次创建同步任务时，在源库中已经指定的将要同步的表，DTS会对这些表自动完成该操作）

    ``` {#codeblock_jfz_f3j_uhp}
    ALTER TABLE schema.table REPLICA IDENTITY FULL;
    ```

-   不支持BIT、VARBIT、GEOMETRY、ARRAY、UUID、TSQUERY、TSVECTOR、TXID\_SNAPSHOT类型的数据同步。
-   RDS PostgreSQL的JSON类型字段可以同步为AnalyticDB for PostgreSQL的VARCHAR类型。
-   仅支持INSERT、UPDATE、DELETE语句的数据同步；不支持DDL及其它DML 语句同步。
-   同步过程中，若源库有涉及同步链路的DDL操作，需要手工在目标库中进行对应的DDL操作，然后重启DTS任务。

## 迁移前准备工作 {#section_2kq_h0l_zm8 .section}

-   在同步启动前，需要在AnalyticDB for PostgreSQL中创建好对应的schema和table。

    **说明：** 暂不支持表结构的同步。

-   源RDS PostgreSQL需要将`wal_level`参数值改为`logical`，操作步骤如下。
    1.  登录[RDS管理控制台](https://rdsnext.console.aliyun.com/#/rdsList/cn-hangzhou/basic/)。
    2.  单击源实例ID或**操作**栏中的**管理**按钮，进入基本信息页面。
    3.  在左侧导航栏，单击**参数设置**。
    4.  在参数设置页面找到`wal_level`参数，将参数值改为`logical`。

        **说明：** 修改`wal_level`参数后需要重启实例才能生效，请评估对业务的影响，在合适的时间进行修改。


## 操作步骤 {#section_qay_4vo_2la .section}

1.  登录[数据传输服务DTS控制台](https://dts.console.aliyun.com/#/home/)。
2.  在左侧导航栏，单击**数据同步**。
3.  在页面右上角，单击**创建同步作业**。
4.  在数据传输服务购买页面，选择付费类型为**预付费**或**按量付费**。
    -   预付费：属于预付费，即在新建实例时需要支付费用。适合长期需求，价格比按量付费更实惠，且购买时长越长，折扣越多。
    -   按量付费：属于后付费，即按小时扣费。适合短期需求，用完可立即释放实例，节省费用。
5.  选择数据同步实例的参数配置信息，参数说明如下表所示。

    |配置项目|配置选项|配置说明|
    |----|----|----|
    |基本配置|功能|选择**数据同步**。|
    |源实例|选择**PostgreSQL**。|
    |源实例地域|选择数据同步链路中源RDS实例的地域。 **说明：** 订购后不支持更换地域，请谨慎选择。

 |
    |目标实例|选择**AnalyticDB for PostgreSQL**。|
    |目标实例地域|选择数据同步链路中目标AnalyticDB for PostgreSQL实例的地域。 **说明：** 订购后不支持更换地域，请谨慎选择。

 |
    |同步拓扑|数据同步支持的拓扑类型，选择**单向同步**。 **说明：** 目前仅支持单向同步。

 |
    |同步链路规格|数据传输为您提供了不同性能的链路规格，以同步的记录数为衡量标准。详情请参见[数据同步规格说明](../../../../cn.zh-CN/产品简介/规格说明/数据同步规格说明.md#)。 **说明：** 建议生产环境选择**small**及以上规格。

 |
    |网络类型|数据同步服务使用的网络类型，目前固定为**专线**。|
    |购买量|购买数量|一次性购买数据同步实例的数量，默认为1，如果购买的是按量付费实例，一次最多购买9 条链路。|

6.  单击**立即购买**，根据提示完成支付流程。
7.  购买成功后单击**管理控制台**。
8.  在左侧导航栏，单击**数据同步**。
9.  定位至已购买的数据同步实例，单击**配置同步链路**。
10. 配置同步通道的源实例及目标实例信息。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/684075/156782485650671_zh-CN.png)

    |配置项目|配置选项|配置说明|
    |----|----|----|
    |同步作业名称|-|     -   DTS为每个任务自动生成一个任务名称，任务名称没有唯一性要求。
    -   您可以根据需要修改任务名称，建议为任务配置具有业务意义的名称，便于后续的任务识别。
 |
    |源实例信息|实例类型|选择**RDS实例**。|
    |实例地区|购买数据同步实例时选择的源实例地域信息，不可变更。|
    |实例ID|选择作为数据同步源的RDS实例ID。|
    |数据库名称|填入同步源表所属的数据库名称。|
    |数据库账号|填入源RDS PostgreSQL实例的数据库账号。 **说明：** 数据库账号须具备schema的owner权限。

 |
    |数据库密码|填入数据库账号对应的密码。|
    |目标实例信息|实例类型|固定为**AnalyticDB for PostgreSQL**，无需设置。|
    |实例地区|购买数据同步实例时选择的目标实例地域信息，不可变更。|
    |实例ID|选择作为数据同步目标的AnalyticDB for PostgreSQL实例ID。|
    |数据库名称|填入同步目标表所属的数据库名称。|
    |数据库账号|填入目标AnalyticDB for PostgreSQL实例的数据库账号。 **说明：** 数据库账号须具备SELECT、INSERT、UPDATE、DELETE、COPY、TRUNCATE、ALTER TABLE权限。

 |
    |数据库密码|填入数据库账号对应的密码。|

11. 单击页面右下角的**授权白名单并进入下一步**。
12. 配置同步策略及对象信息。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/684075/156782485752438_zh-CN.png)

    |配置项目|配置参数|配置说明|
    |:---|----|:---|
    |同步策略配置|同步初始化|选择**全量数据初始化**。 **说明：** 将源实例中已经存在同步对象的数据在目标实例中初始化，作为后续增量同步数据的基线数据。

 |
    |目标已存在表的处理模式|     -   **预检查并报错拦截**：如果发现目标表中已有数据，报错停止。
    -   **清空目标表数据**：如果发现目标表中已有数据，清空表数据。
    -   **无操作**：无论目标表是否有数据，不做任何操作。
 |
    |同步操作类型|     -   **Insert**
    -   **Update**
    -   **Delete**
    -   **AlterTable**
 **说明：** 

    -   RDS PostgreSQL迁移到AnalyticDB for PostgreSQL不支持**AlterTable**同步操作。
    -   根据业务需求选择数据同步的操作类型。
 |
    |选择同步对象|-|同步对象的选择粒度为表。 如果需要目标表中列信息与源表不同，则需要使用DTS的字段映射功能，详情请参见[库表列映射](https://help.aliyun.com/document_detail/26628.html)。

 **说明：** 不支持CREATE TABLE操作，您需要通过修改同步对象操作来新增对应表的同步，详情请参见[新增同步对象](https://help.aliyun.com/document_detail/26634.html)。

 |

13. 上述配置完成后单击页面右下角的**预检查并启动**。

    **说明：** 

    -   在数据同步任务正式启动之前，会先进行预检查。只有预检查通过后，才能成功启动数据同步任务。
    -   如果预检查失败，请查看具体的失败详情。根据失败原因修复后，重新进行预检查。
14. 在预检查对话框中显示**预检查通过**后，关闭预检查对话框，该同步作业的同步任务正式开始。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/684075/156782485750676_zh-CN.png)

15. 等待该同步作业的链路初始化完成，直至状态处于**同步中**。

    您可以在 数据同步页面，查看数据同步状态。


