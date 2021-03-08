# UpgradeDBInstance

You can call this operation to change the type of an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=UpgradeDBInstance&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|DBInstanceClass|String|Yes|gpdb.group.segsdx1|The instance type. For more information, see [Instance types](https://help.aliyun.com/document_detail/86942.html?spm=a2c4g.11186623.2.14.326b72932oCiGD#concept-d1p-13m-q2b). |
|DBInstanceGroupCount|String|Yes|2|The number of compute nodes of the instance. |
|DBInstanceId|String|Yes|gp-xxxxxxxx|The ID of the instance. |
|RegionId|String|Yes|cn-hangzhou|The region ID of the instance. |
|PayType|String|No|Prepaid|The billing method of the instance. Default value: Postpaid. Valid values:

-   Postpaid: pay-as-you-go
-   Prepaid: subscription |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|25C11EE5-B7E8-481A-A07C-BD619971A570|The ID of the request. |
|DBInstanceId|String|gp-xxxxxxxxxx|The ID of the instance. |
|OrderId|String|101450956|The order ID of the instance. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=UpgradeDBInstance
&DBInstanceClass=gpdb.group.segsdx1
&DBInstanceGroupCount=2
&DBInstanceId=gp-xxxxxxxx
&RegionId=cn-hangzhou
&<Common request parameters>
```

Sample success responses

`XML` format

```
<UpgradeDBInstanceResponse>
          <requestId>25C11EE5-B7E8-481A-A07C-BD619971A570</requestId>
          <DBInstanceId>gp-wz9kmr708m155j***</DBInstanceId>
          <OrderId>101450956</OrderId>
</UpgradeDBInstanceResponse>
```

`JSON` format

```
{
  "requestId": "25C11EE5-B7E8-481A-A07C-BD619971A570",
  "DBInstanceId": "gp-wz9kmr708m155j***",
  "OrderId": "101450956"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

