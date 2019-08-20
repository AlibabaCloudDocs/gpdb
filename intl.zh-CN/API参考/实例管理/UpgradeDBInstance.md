# UpgradeDBInstance {#concept_1796507 .concept}

## 描述 {#section_zut_g9h_gbb .section}

调用UpgradeDBInstance变更分析型数据库PostgreSQL版的实例规格。

**说明：** 

-   进行变更配置操作时，实例需处于运行中状态，否则将操作失败。
-   根据数据量不同，实例规格升级的过程大约需要30分钟到3个小时。
-   为了保证数据的一致性，变更过程中实例仅提供只读服务，并且会出现闪断两次，请您提前对业务做出调整。
-   规格变更完成后，实例的数据库内核版本自动升级为最新。

该API对应的控制台操作请参见[升级实例规格](../../../../intl.zh-CN/用户指南/管理实例/升级实例规格.md#section_g4v_xkz_52b)。

## 请求参数 {#section_qtu_6ty_2hk .section}

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|UpgradeDBInstance|系统规定参数。取值：UpgradeDBInstance。|
|RegionId|String|是|cn-hangzhou|实例所属的地域ID。可调用[DescribeRegions](intl.zh-CN/API参考/实例管理/DescribeRegions.md#)查询。|
|DBInstanceId|String|是|gp-xxxxxxxxxxxxx|待变更配置的实例ID。|
|DBInstanceClass|String|是|gpdb.group.segsdx1|实例规格，详情请参见[实例规格表](intl.zh-CN/API参考/附录/实例规格表.md#)。|
|DBInstanceGroupCount|String|是|2|分析型数据库PostgreSQL版的计算组数量。|

## 返回参数 {#section_k00_14x_5uy .section}

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|25C11EE5-B7E8-481A-A07C-BD619971A570|请求ID。|

## 请求示例 {#section_5s4_fig_c4b .section}

``` {#codeblock_cbn_zkd_vl8}
请求示例https://gpdb.aliyuncs.com/?Action=UpgradeDBInstance
&DBInstanceId=gp-wz9kmr708m155j***
&DBInstanceClass=gpdb.group.segsdx1
&DBInstanceGroupCount=2
&<公共请求参数>
```

## 返回示例 {#section_rrb_ip1_1o6 .section}

``` {#codeblock_o5k_j45_5rx}
{
  "requestId": "25C11EE5-B7E8-481A-A07C-BD619971A570"
}
```

