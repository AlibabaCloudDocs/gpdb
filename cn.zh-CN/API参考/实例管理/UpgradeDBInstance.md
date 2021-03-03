# UpgradeDBInstance

调用UpgradeDBInstance变更分析型数据库PostgreSQL版的实例规格。

## 调试

[您可以在OpenAPI Explorer中直接运行该接口，免去您计算签名的困扰。运行成功后，OpenAPI Explorer可以自动生成SDK代码示例。](https://api.aliyun.com/#product=gpdb&api=UpgradeDBInstance&type=RPC&version=2016-05-03)

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|DBInstanceClass|String|是|gpdb.group.segsdx1|实例规格，详情请参见[实例规格表](https://help.aliyun.com/document_detail/86942.html?spm=a2c4g.11186623.2.14.326b72932oCiGD#concept-d1p-13m-q2b)。 |
|DBInstanceGroupCount|String|是|2|分析型数据库PostgreSQL版的计算组数量。 |
|DBInstanceId|String|是|gp-xxxxxxxx|实例ID。 |
|RegionId|String|是|cn-hangzhou|地域ID。 |
|PayType|String|否|Prepaid|付费类型：

-   Postpaid：按量付费，为默认值。
-   Prepaid：包年包月。 |

## 返回数据

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|25C11EE5-B7E8-481A-A07C-BD619971A570|请求ID。 |
|DBInstanceId|String|gp-xxxxxxxxxx|实例ID。 |
|OrderId|String|101450956|订单ID， |

## 示例

请求示例

```
https://gpdb.aliyuncs.com/?Action=UpgradeDBInstance
&DBInstanceClass=gpdb.group.segsdx1
&DBInstanceGroupCount=2
&DBInstanceId=gp-xxxxxxxx
&RegionId=cn-hangzhou
&<公共请求参数>
```

正常返回示例

`XML` 格式

```
<UpgradeDBInstanceResponse>
          <requestId>25C11EE5-B7E8-481A-A07C-BD619971A570</requestId>
          <DBInstanceId>gp-wz9kmr708m155j***</DBInstanceId>
          <OrderId>101450956</OrderId>
</UpgradeDBInstanceResponse>
```

`JSON` 格式

```
{
  "requestId": "25C11EE5-B7E8-481A-A07C-BD619971A570",
  "DBInstanceId": "gp-wz9kmr708m155j***",
  "OrderId": "101450956"
}
```

## 错误码

访问[错误中心](https://error-center.aliyun.com/status/product/gpdb)查看更多错误码。

