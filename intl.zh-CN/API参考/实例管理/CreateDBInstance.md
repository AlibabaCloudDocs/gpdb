# CreateDBInstance {#concept_nkf_dbm_q2b .concept}

## 描述 { .section}

创建HybridDB for PostgreSQL实例。

## 输入参数 { .section}

|名称|类型|是否必须|描述|
|--|--|----|--|
|<公共请求参数\>|-|是|参见[公共参数](intl.zh-CN/API参考/公共参数.md#)。|
|Action|String|是|CreateDBInstance|
|RegionId|String|是|地域ID，如cn-hangzhou，可选的地域详见DescribeRegions接口。|
|ZoneId|String|是|可用区ID，如cn-hangzhou-d，可选的可用区详见DescribeRegions接口。|
|Engine|String|是|引擎，取值为gpdb。|
|EngineVersion|String|是|引擎版本，取值为4.3。|
|PayType|String|否|付费类型：-   Postpaid：按量付费，为默认值。
-   Prepaid：包年包月。

|
|InstanceNetworkType|String|是|实例网络类型，取值为Classic或者VPC，默认为"Classic"。|
|VPCId|String|否|InstanceNetworkType=VPC时，VPCId 必须填写，VPCId所在地域必须与RegionId 保持一致。|
|VSwitchId|String|否|InstanceNetworkType=VPC时，VSwitchId 必须填写，VSwitchId所在可用区必须与ZoneId 保持一致。|
|DBInstanceClass|String|是|实例规格，详见[实例规格表](intl.zh-CN/API参考/附录/实例规格表.md#)。|
|DBInstanceGroupCount|String|是|HybridDB for PostgreSQL 计算组的数量。|
|DBInstanceDescription|String|否|HybridDB for PostgreSQL实例描述，长度为256字符。|
|SecurityIPList|String|否|IP白名单，默认值为"`127.0.0.1`"。|
|ClientToken|String|是|幂等性校验。|

## 返回参数 { .section}

|参数|类型|说明|
|--|--|--|
|<公共返回参数\>|String|详见[公共返回参数](intl.zh-CN/API参考/公共参数.md#section_apd_1rv_3bb)|
|DBInstanceId|String|实例名|
|OrderId|String|订单ID|

## 请求示例 {#section_uvb_mmt_42b .section}

```
https://gpdb.aliyuncs.com/?Action=CreateDBInstance
&Engine=gpdb
&EngineVersion=4.3
&RegionId=cn-hangzhou
&ZoneId=cn-hangzhou-b
&PayType=Postpaid
&InstanceNetworkType=Classic
&DBInstanceClass=gpdb.group.segsdx1
&DBInstanceGroupCount=2
&ClientToken=f918f59c-89be-42ee-bb86-d027243b2cfb
&<公共请求参数>
```

## 返回示例 { .section}

**XML格式**

```
<CreateDBInstanceResponse>
  <dbInstanceId>gp-xxxxxxx</dbInstanceId>
  <orderId>xxxxxxxxxxx</orderId>
</CreateDBInstanceResponse>
```

**JSON格式**

```
{
	"dbInstanceId":"gp-xxxxxxx",
	"orderId":xxxxxxxxxxx
}
```

