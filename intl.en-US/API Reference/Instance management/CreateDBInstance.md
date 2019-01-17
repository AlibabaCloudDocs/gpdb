# CreateDBInstance {#concept_nkf_dbm_q2b .concept}

## Description { .section}

You can call this operation to create an instance of HybridDB for PostgreSQL.

## Request parameters { .section}

|Name|Type|Required| Description|
|----|----|--------|------------|
|<Common request parameters\>|-| Yes|For more information, see [Common parameters](reseller.en-US/API Reference/Common parameters.md#).|
|Action|String|Yes|The operation that you want to perform. Set the value to CreateDBInstance.|
|RegionId|String|Yes|The ID of a region, such as cn-hangzhou. For more information about available regions, see DescribeRegions.|
|ZoneId|String|Yes|The ID of a zone, such as cn-hangzhou-d. For more information about available zones, see DescribeRegions.|
|Engine|String|Yes|The Engine of the database, and the value is gpdb.|
|EngineVersion|String|Yes|The version of engine, and the value is 4.3.|
|PayType|String|No|The billing methods:-   Postpaid: Pay-As-You-Go. The default value is Postpaid.
-   Prepaid: Subscription.

|
|InstanceNetworkType|String| Yes|The network type of an instance. Valid values: Classic and VPC. Default value: Classic.|
|VPCId| String| No|When you specify VPC for InstanceNetworkType, you must specify VPCId. The region of the VPC must be the same as that specified by RegionID.|
|VSwitchId|String|No|When you specify VPC for InstanceNetworkType, you must specify VSwitchId. The zone of the VSwitch must be the same as that specified by ZoneId.|
|DBInstanceClass|String| Yes|The type of a group. For more information, see[Instance types](reseller.en-US/API Reference/Appendix/Instance types.md#).|
|DBInstanceGroupCount|String|Yes|The number of groups.|
|DBInstanceDescription| String| No|The description of the instance. The maximum length is 256 characters.|
|SecurityIPList| String| No|The whitelist. Default value: `127.0.0.1`.|
|ClientToken|String| Yes|The idempotence check.|

## Response parameters { .section}

|Name|Type |Description|
|----|-----|-----------|
|<Common response parameters\>|String|For more information, see [Common response parameters](reseller.en-US/API Reference/Common parameters.md#section_apd_1rv_3bb)|
|DBInstanceId|String |The instance ID.|
|OrderId|String |The order ID.|

## Sample requests {#section_uvb_mmt_42b .section}

```
https://gpdb.aliyuncs.com/?Action=CreateDBInstance
&Engine=gpdb 
&EngineVersion=4.3
&amp;RegionId=cn-hangzhou
&ZoneId=cn-hangzhou-b
&PayType=Postpaid
&InstanceNetworkType=Classic
&DBInstanceClass=gpdb.group.segsdx1
&DBInstanceGroupCount=2
&ClientToken=f918f59c-89be-42ee-bb86-d027243b2cfb
&<Common request parameters>
```

## Sample responses { .section}

**XML format**

```
<CreateDBInstanceResponse>
  <dbInstanceId>gp-xxxxxxx</dbInstanceId>
  <orderId>202292238780941</orderId>
</CreateDBInstanceResponse>
```

**JSON format**

```
{
	"dbInstanceId":"gp-xxxxxxx",
	"orderId":202292238780941
}
```

