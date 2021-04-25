# ModifyDBInstanceSSL

You can call this operation to enable, disable, and update SSL encryption.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=ModifyDBInstanceSSL&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|ModifyDBInstanceSSL|The operation that you want to perform. Set the value to ModifyDBInstanceSSL. |
|ConnectionString|String|Yes|gp-t4ni14frt6lhjk74a-master.gpdbmaster.singapore.rds.aliyuncs.com|The encrypted endpoint. By default, the wildcards are used for instances that are hosted on ECS instances. This way, the endpoints that can be resolved to the same IP address are encrypted. |
|DBInstanceId|String|Yes|gp-xxxxxxxxxxx|The ID of the instance. |
|SSLEnabled|Integer|Yes|1|The status of SSL encryption. Valid values:

 -   0: disables SSL encryption.
-   1: enables SSL encryption.
-   2: updates SSL encryption. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|ADD6EA90-EECB-4C12-9F26-0B6DB58710EF|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=ModifyDBInstanceSSL
&ConnectionString=gp-t4ni14frt6lhjk74a-master.gpdbmaster.singapore.rds.aliyuncs.com
&DBInstanceId=gp-xxxxxxxxxxx
&SSLEnabled=1
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>ADD6EA90-EECB-4C12-9F26-0B6DB58710EF</RequestId>
```

`JSON` format

```
{"RequestId":"ADD6EA90-EECB-4C12-9F26-0B6DB58710EF"}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

