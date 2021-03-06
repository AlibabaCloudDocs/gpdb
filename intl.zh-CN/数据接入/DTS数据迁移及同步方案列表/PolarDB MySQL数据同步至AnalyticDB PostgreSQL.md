# PolarDB MySQL数据同步至AnalyticDB PostgreSQL

数据传输服务DTS（Data Transmission Service）支持将PolarDB MySQL数据同步至AnalyticDB PostgreSQL，帮助您轻松实现数据的流转，将企业数据集中分析。

-   PolarDB MySQL集群已开启Binlog，详情请参见[如何开启Binlog](https://www.alibabacloud.com/help/zh/doc-detail/113546.htm)。
-   PolarDB MySQL集群中待同步的数据表必须具备主键。
-   已创建目标云原生数据仓库AnalyticDB PostgreSQL实例，详情请参见[创建云原生数据仓库AnalyticDB PostgreSQL实例](https://www.alibabacloud.com/help/zh/doc-detail/50200.htm)。

## 注意事项

-   DTS在执行全量数据初始化时将占用源库和目标库一定的读写资源，可能会导致数据库的负载上升，在数据库性能较差、规格较低或业务量较大的情况下（例如源库有大量慢SQL、存在无主键表或目标库存在死锁等），可能会加重数据库压力，甚至导致数据库服务不可用。因此您需要在执行数据同步前评估源库和目标库的性能，同时建议您在业务低峰期执行数据同步（例如源库和目标库的CPU负载在30%以下）。
-   全量初始化过程中，并发INSERT会导致目标实例的表碎片，全量初始化完成后，目标实例的表空间比源集群的表空间大。

## 同步限制

-   同步对象仅支持数据表。
-   不支持BIT、VARBIT、GEOMETRY、ARRAY、UUID、TSQUERY、TSVECTOR、TXID\_SNAPSHOT类型的数据同步。
-   暂不支持同步前缀索引，如果源库存在前缀索引可能导致数据同步失败。
-   在数据同步时，请勿对源库的同步对象使用gh-ost或pt-online-schema-change等类似工具执行在线DDL变更，否则会导致同步失败。

## 支持同步的SQL操作

-   DML操作：INSERT、UPDATE、DELETE。
-   DDL操作：ADD COLUMN。

    **说明：** 不支持CREATE TABLE操作，如果您需要将新增的表作为同步对象，则需要执行[新增同步对象](/intl.zh-CN/数据同步/同步作业管理/新增同步对象.md)操作。


## 支持的同步架构

-   1对1单向同步。
-   1对多单向同步。
-   多对1单向同步。

## 术语对应关系

|PolarDB MySQL|云原生数据仓库AnalyticDB PostgreSQL|
|:------------|:---------------------------|
|Database|Schema|
|Table|Table|

## 操作步骤

1.  购买数据同步作业，详情请参见[购买流程]()。

    **说明：** 购买时，选择源实例为**PolarDB**、目标实例为**AnalyticDB PostgreSQL**，并选择同步拓扑为**单向同步**。

2.  登录[数据传输控制台](https://dts-intl.console.aliyun.com/)。

3.  在左侧导航栏，单击**数据同步**。

4.  在同步作业列表页面顶部，选择同步的目标实例所属地域。

    ![选择地域](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9699340261/p50604.png)

5.  定位至已购买的数据同步实例，单击**配置同步链路**。

6.  配置同步通道的源实例及目标实例信息。

    ![配置源和目标实例信息](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1130649951/p103968.png)

    |类别|配置|说明|
    |:-|:-|:-|
    |无|同步作业名称|DTS会自动生成一个同步作业名称，建议配置具有业务意义的名称（无唯一性要求），便于后续识别。|
    |源实例信息|实例类型|固定为**PolarDB实例**。|
    |实例地区|购买数据同步实例时选择的源PolarDB集群的地域信息，不可变更。|
    |PolarDB实例ID|选择PolarDB集群ID。|
    |数据库账号|填入PolarDB集群的数据库账号。 **说明：** 该账号需具备待同步对象的读权限。 |
    |数据库密码|填入该数据库账号的密码。|
    |目标实例信息|实例类型|固定为**AnalyticDB for PostgreSQL**，无需设置。|
    |实例地区|购买数据同步实例时选择的目标实例地域信息，不可变更。|
    |实例ID|选择云原生数据仓库AnalyticDB PostgreSQL实例ID。|
    |数据库名称|填入云原生数据仓库AnalyticDB PostgreSQL实例中，待同步的目标表所属的数据库名称。|
    |数据库账号|填入云原生数据仓库AnalyticDB PostgreSQL的**初始账号**，详情请参见[t16843.md\#](/intl.zh-CN/快速入门/创建数据库账号.md)。 **说明：** 您也可以填入具备RDS\_SUPERUSER权限的账号，创建方法请参见[用户权限管理](/intl.zh-CN/开发入门/用户权限管理.md)。 |
    |数据库密码|填入数据库账号的密码。|

7.  单击页面右下角的**授权白名单并进入下一步**。

    **说明：** 此步骤会将DTS服务器的IP地址自动添加PolarDB MySQL和云原生数据仓库AnalyticDB PostgreSQL的白名单中，用于保障DTS服务器能够正常连接源集群和目标实例。

8.  配置同步策略及同步对象。

    ![mysql同步至ADB4PG](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7310431261/p96654.png)

    |类别|配置|说明|
    |:-|:-|:-|
    |同步策略配置|同步初始化|默认情况下，您需要同时选中**结构初始化**和**全量数据初始化**。预检查完成后，DTS会将源实例中待同步对象的结构及数据在目标实例中初始化，作为后续增量同步数据的基线数据。|
    |目标已存在表的处理模式|    -   **清空目标表的数据**

在预检查阶段跳过**同名对象存在性检查**的检查项目。全量初始化之前将目标表的数据清空。适用于完成同步任务测试后的正式同步场景。

    -   **忽略报错并继续执行**

在预检查阶段跳过**同名对象存在性检查**的检查项目。全量初始化时直接追加数据。适用于多张表同步到一张表的汇总同步场景。 |
    |同步操作类型|根据业务需求选择需要同步的操作类型：

    -   **Insert**
    -   **Update**
    -   **Delete**
    -   **AlterTable** |
    |选择同步对象|无|在源库对象框中单击待同步的表，然后单击![向右小箭头](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8502659951/p40698.png)图标将其移动至已选择对象框。

**说明：**

    -   同步对象的选择粒度为表。
    -   如果需要目标表中的列名称与源表不同，需要使用DTS的字段映射功能，详情请参见[设置同步对象在目标实例中的名称](/intl.zh-CN/数据同步/同步作业管理/设置同步对象在目标实例中的名称.md)。 |
    |映射名称更改|无|如需更改同步对象在目标实例中的名称，请使用对象名映射功能，详情请参见[库表列映射](/intl.zh-CN/数据迁移/迁移任务管理/库表列映射.md)。 |
    |源表DMS\_ONLINE\_DDL过程中是否复制临时表到目标库|无|如源库使用[数据管理DMS（Data Management Service）]()执行Online DDL变更，您可以选择是否同步Online DDL变更产生的临时表数据。

    -   **是**：同步Online DDL变更产生的临时表数据。

**说明：** Online DDL变更产生的临时表数据过大，可能会导致同步任务延迟。

    -   **否**：不同步Online DDL变更产生的临时表数据，只同步源库的原始DDL数据。

**说明：** 该方案会导致目标库锁表。 |
    |源、目标库无法连接重试时间|无|当源、目标库无法连接时，DTS默认重试720分钟（即12小时），您也可以自定义重试时间。如果DTS在设置的时间内重新连接上源、目标库，同步任务将自动恢复。否则，同步任务将失败。

**说明：** 由于连接重试期间，DTS将收取任务运行费用，建议您根据业务需要自定义重试时间，或者在源和目标库实例释放后尽快释放DTS实例。 |

9.  设置待同步的表在云原生数据仓库AnalyticDB PostgreSQL中的主键列和分布列信息。

    ![](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4330649951/p65402.png)

    **说明：** 当您在上一步中选择了**结构初始化**才会出现该页面。关于主键列和分布列的详细说明，请参见[表的约束定义](https://www.alibabacloud.com/help/zh/doc-detail/118150.htm#title-8zk-q8o-q3l)和[表分布键定义](https://www.alibabacloud.com/help/zh/doc-detail/120143.htm)。

10. 上述配置完成后，单击页面右下角的**预检查并启动**。

    **说明：**

    -   在同步作业正式启动之前，会先进行预检查。只有预检查通过后，才能成功启动同步作业。
    -   如果预检查失败，单击具体检查项后的![提示](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8502659951/p47468.png)，查看失败详情。
        -   您可以根据提示修复后重新进行预检查。
        -   如无需修复告警检测项，您也可以选择**确认屏蔽**、**忽略告警项并重新进行预检查**，跳过告警检测项重新进行预检查。
11. 在预检查对话框中显示**预检查通过**后，关闭预检查对话框，同步作业将正式开始。

12. 等待同步作业的链路初始化完成，直至处于**同步中**状态。

    您可以在数据同步页面，查看数据同步作业的状态。

    ![查看同步作业状态](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6344738161/p41059.png)


