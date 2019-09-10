# Tableau连接分析型数据库PostgreSQL版 {#concept_960831 .concept}

Tableau是一款数据分析与可视化工具，支持连接本地或云端数据，无论是电子表格还是数据库数据，都可以无缝连接。本文介绍使用Tableau连接分析型数据库PostgreSQL版。

## 连接Tableau {#section_4kr_eix_2vh .section}

1.  启动Tableau。
2.  在连接页面选择**Pivotal Greenplum Database**。
3.  在登陆页面填写数据库连接信息后单击**登录**。

    **说明：** 若连接失败请确认数据库连接信息是否正确，检查数据库白名单是否添加Tableau所在服务器IP地址，确认无误后重新登录。如何添加白名单请参见[设置白名单](intl.zh-CN/用户指南/管理实例/设置白名单.md#)。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/776343/156809172451418_zh-CN.png)

    |配置项|描述|
    |---|--|
    |服务器|分析型数据库PostgreSQL版外网连接地址。详情请参见[申请外网地址](intl.zh-CN/用户指南/管理实例/申请外网地址.md#)。|
    |端口|默认为3432。|
    |数据库|分析型数据库PostgreSQL版数据库名称。|
    |用户名|分析型数据库PostgreSQL版数据库账户。|
    |密码|分析型数据库PostgreSQL版数据库密码。|

4.  成功登录后页面如下所示。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/776343/156809172451281_zh-CN.png)


## 最佳实践 {#section_fhb_m58_ct0 .section}

**统计分析**

根据指导操作，可以对任意表进行统计分析，并进行报表展示。

例如使用TPCH数据中的lineitem，点开一张工作表可以进行任意维度的数据展示。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/776343/156809172551283_zh-CN.png)

当您从度量或者维度中选择字段，放到工作表区时，Tableau都会发送一个query到分析型数据库PostgreSQL版进行数据查询，例如上述图表发送的query如下所示：

``` {#codeblock_btq_yla_4lr}
BEGIN;declare "SQL_CUR0x7fdabf04ca00" cursor with hold for SELECT "lineitem"."l_linestatus" AS "l_linestatus",
          "lineitem"."l_shipmode" AS "l_shipmode",
          SUM("lineitem"."l_orderkey") AS "sum_l_orderkey_ok",
          ((CAST("lineitem"."l_shipdate" AS DATE) + CAST(TRUNC((-1 * (EXTRACT(DAY FROM "lineitem"."l_shipdate") - 1))) AS INTEGER) * INTERVAL '1 DAY') + CAST(TRUNC((-1 * (EXTRACT(MONTH FROM "lineitem"."l_shipdate") - 1))) AS INTEGER) * INTERVAL '1 MONTH') AS "tyr_l_shipdate_ok"
        FROM "public"."lineitem" "lineitem"
        GROUP BY 1,
          2,
          4;fetch 10000 in "SQL_CUR0x7fdabf04ca00
```

**关闭cursor**

默认情况下Tableau使用cursor模式从分析型数据库PostgreSQL版拉取数据：

``` {#codeblock_udx_61v_7r4}
FETCH 10000 in “SQL_CUR0x7fe678049e00”
```

如果提取的数据量很大，并且Tableau服务器的内存足够放下所有的查询数据，可以通过关闭cursor模式进行性能调优。

操作步骤如下：

1.  创建关闭cursor模式TDC文件，文件配置信息如下：

    ``` {#codeblock_t3u_vaw_1j6}
    <?xml version='1.0' encoding='utf-8' ?>  
    <connection-customization class='greenplum' enabled='true' version='4.3'>  
    <vendor name='greenplum'/>  
    <driver name='greenplum'/>  
    <customizations>  
    <customization name='odbc-connect-string-extras' value='UseDeclareFetch=0' />
    </customizations>  
    </connection-customization>
    ```

2.  将该文件以tdc为后缀名，Desktop版本的Tableau放到`Documents/My Tableau Repository/Datasources`目录下，其他版本的Tableau同样放置到对应的Datasources目录下。
3.  重启Tableau即可生效。

也可以修改fetch的size值，让其每次fetch更多的数据：

``` {#codeblock_rgk_xr7_dyh}
<?xml version='1.0' encoding='utf-8' ?>  
<connection-customization class='greenplum' enabled='true' version='4.3'>  
<vendor name='greenplum'/>  
<driver name='greenplum'/>  
<customizations>  
<customization name='odbc-connect-string-extras' value='Fetch=100000' />  
</customizations>  
</connection-customization>
```

**初始化SQL**

连接建立时可以通过初始化SQL设置特定参数，如下图所示。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/776343/156809172551286_zh-CN.png)

**说明：** SQL语言结尾请不要添加英文分号（;），Tableau会将该SQL封装执行，中间如果有分号会报语法错误。同样在自定义SQL时，SQL结尾也不能加（;）。

