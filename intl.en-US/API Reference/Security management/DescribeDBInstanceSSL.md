# DescribeDBInstanceSSL

You can call this operation to query SSL information of an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DescribeDBInstanceSSL&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DescribeDBInstanceSSL|The operation that you want to perform. Set the value to DescribeDBInstanceSSL. |
|DBInstanceId|String|Yes|gp-xxxxxxxx|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|CertCommonName|String|\*.gpdbmaster.xxx.rds.aliyuncs.com|The name of the SSL certificate. |
|DBInstanceId|String|gp-xxxxx|The ID of the instance. |
|DBInstanceName|String|gp-xxxxx|The name of the instance. |
|RequestId|String|D5FF8636-37F6-4CE0-8002-F8734C62C686|The ID of the request. |
|SSLEnabled|Boolean|true|Indicates whether SSL encryption is enabled. |
|SSLExpiredTime|String|2020-08-05T09:05:53Z|The expiration time of the SSL certificate. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/?Action=DescribeDBInstanceSSL
&<Common request parameters>
&DBInstanceId=gp-xxxxxx
```

Sample success responses

`XML` format

```
<SSLEnabled>true</SSLEnabled>
<RequestId>D5FF8636-37F6-4CE0-8002-F8734C62C686</RequestId>
<DBInstanceName>gp-xxx</DBInstanceName>
```

`JSON` format

```
{
  "SSLEnabled": true,
  "RequestId": "D5FF8636-37F6-4CE0-8002-F8734C62C686",
  "DBInstanceName": "gp-xxx"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

