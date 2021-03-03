# RDS PostgreSQL实时同步数据至AnalyticDB for PostgreSQL

数据传输服务DTS（Data Transmission Service）支持将RDS PostgreSQL同步至AnalyticDB for PostgreSQL。通过DTS提供的数据同步功能，可以轻松实现数据的流转，将企业数据集中分析。

-   RDS PostgreSQL中待同步的数据表必须具备主键。
-   已创建目标云原生数据仓库AnalyticDB PostgreSQL实例，如未创建请参见[创建云原生数据仓库AnalyticDB PostgreSQL实例](https://help.aliyun.com/document_detail/50200.html)。

## 注意事项

-   一个数据同步作业只能同步一个数据库，如果有多个数据库需要同步，则需要为每个数据库创建数据同步作业。
-   在数据同步的过程中，如果要将源库中创建的新表作为同步对象，您需要对该表执行如下操作以保障该表数据同步的一致性。

    ```
    ALTER TABLE schema.table REPLICA IDENTITY FULL;
    ```


## 同步限制

-   不支持结构初始化，即不支持将源库中待同步对象的结构定义（例如表结构）同步至目标库中。
-   同步对象仅支持数据表。
-   不支持BIT、VARBIT、GEOMETRY、ARRAY、UUID、TSQUERY、TSVECTOR、TXID\_SNAPSHOT类型的数据同步。
-   同步过程中，如果对源库中的同步对象执行了DDL操作，需要手动在目标库中执行对应的DDL操作，然后重启数据同步作业。

## 支持的同步语法

仅支持INSERT、UPDATE、DELETE。

## 准备工作

1.  调整源RDS实例的`wal_level`参数设置。

    **警告：** 修改`wal_level`参数后需要重启实例才能生效，请评估对业务的影响，在业务低峰期进行修改。

    1.  登录[RDS管理控制台](https://rdsnext.console.aliyun.com/#/rdsList/cn-hangzhou/basic/)。

    2.  在页面左上角，选择实例所在地域。

    3.  找到目标实例，单击实例ID。

    4.  在左侧导航栏，单击**参数设置**。

    5.  在参数设置页面找到`wal_level`参数，将参数值改为`logical`。

2.  根据源RDS实例中待同步对象的结构，在目标云原生数据仓库AnalyticDB PostgreSQL中创建相应的数据库、Schema、表等结构信息，详情请参见[SQL语法](https://help.aliyun.com/document_detail/118877.html)。


## 操作步骤

1.  购买数据同步作业，详情请参见[购买流程](/cn.zh-CN/快速入门/购买流程.md)。

    **说明：** 购买时，选择源实例为**PostgreSQL**、目标实例为**AnalyticDB for PostgreSQL**，并选择同步拓扑为**单向同步**。

2.  登录[数据传输控制台](https://dts.console.aliyun.com/)。

3.  在左侧导航栏，单击**数据同步**。

4.  在同步作业列表页面顶部，选择同步的目标实例所属地域。

    ![选择地域](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/7349459951/p50604.png)

5.  定位至已购买的数据同步实例，单击**配置同步链路**。

6.  配置同步作业的源实例及目标实例信息。

    ![配置源和目标库信息](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/4330649951/p63355.png)

    |类别|配置|说明|
    |--|--|--|
    |无|同步作业名称|DTS会自动生成一个同步作业名称，建议配置具有业务意义的名称（无唯一性要求），便于后续识别。|
    |源实例信息|实例类型|选择**RDS实例**。|
    |实例地区|购买数据同步实例时选择的源实例地域信息，不可变更。|
    |实例ID|选择RDS PostgreSQL实例ID。|
    |数据库名称|填入待同步的表所属的数据库名称。|
    |数据库账号|填入RDS PostgreSQL的数据库账号。 **说明：** 数据库账号须具备schema的owner权限。 |
    |数据库密码|填入该数据库账号对应的密码。|
    |目标实例信息|实例类型|固定为**AnalyticDB for PostgreSQL**，无需设置。|
    |实例地区|购买数据同步实例时选择的目标实例地域信息，不可变更。|
    |实例ID|选择云原生数据仓库AnalyticDB PostgreSQL实例ID。|
    |数据库名称|填入同步目标表所属的数据库名称。 **说明：** 该库须在云原生数据仓库AnalyticDB PostgreSQL中存在，如不存在请[创建数据库](https://help.aliyun.com/document_detail/118127.html)。 |
    |数据库账号|填入云原生数据仓库AnalyticDB PostgreSQL的**初始账号**，详情请参见[t16843.md\#](/cn.zh-CN/快速入门/设置账号.md)。 **说明：** 您也可以填入具备RDS\_SUPERUSER权限的账号，创建方法请参见[用户权限管理](/cn.zh-CN/开发入门/用户权限管理.md)。 |
    |数据库密码|填入该数据库账号对应的密码。|

7.  单击页面右下角的**授权白名单并进入下一步**。

    **说明：** 此步骤会将DTS服务器的IP地址自动添加到RDS PostgreSQL和云原生数据仓库AnalyticDB PostgreSQL的白名单中，用于保障DTS服务器能够正常连接源和目标实例。

8.  配置同步策略及对象信息。

    ![选择同步对象](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/6330649951/p99535.png)

    |类别|配置|说明|
    |:-|:-|:-|
    |同步策略配置|同步初始化|默认情况下，您需要勾选**全量数据初始化**。预检查完成后，DTS会将源实例中待同步对象的存量数据同步至目标实例，作为后续增量同步数据的基线数据。|
    |目标已存在表的处理模式|    -   **清空目标表的数据**

在预检查阶段跳过**目标表是否为空**的检查项目。全量初始化之前将目标表的数据清空。适用于完成同步任务测试后的正式同步场景。

    -   **忽略报错并继续执行**

在预检查阶段跳过**目标表是否为空**的检查项目。全量初始化时直接追加迁移数据。适用于多张表同步到一张表的汇总同步场景。 |
    |同步操作类型|根据业务需求选择需要同步的操作类型：

**说明：** 不支持**AlterTable**。

    -   **Insert**
    -   **Update**
    -   **Delete**
    -   **AlterTable** |
    |选择同步对象|无|在源库对象框中单击待同步的表，然后单击![向右小箭头](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8502659951/p40698.png)将其移动至已选择对象框。

**说明：**

    -   同步对象的选择粒度为表。
    -   如果需要目标表中的列名称与源表不同，则需要使用DTS的字段映射功能，详情请参见[设置同步对象在目标实例中的名称](/cn.zh-CN/数据同步/同步作业管理/设置同步对象在目标实例中的名称.md)。 |

9.  上述配置完成后，单击页面右下角的**预检查并启动**。

    **说明：**

    -   在数据同步作业正式启动之前，会先进行预检查。只有预检查通过后，才能成功启动数据同步作业。
    -   如果预检查失败，单击具体检查项后的![提示](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/8502659951/p47468.png)图标，查看失败详情。根据提示修复后，重新进行预检查。
10. 在预检查对话框中显示**预检查通过**后，关闭预检查对话框，同步作业将正式开始。

11. 等待同步作业的链路初始化完成，直至处于**同步中**状态。

    您可以在数据同步页面，查看数据同步作业的状态。

    ![查看同步作业状态](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1349459951/p41059.png)


