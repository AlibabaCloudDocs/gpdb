# AllocateInstancePublicConnection

You can call this operation to specify a public endpoint for an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=AllocateInstancePublicConnection&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|No|AllocateInstancePublicConnection|The operation that you want to perform. Set the value to AllocateInstancePublicConnection. |
|ConnectionStringPrefix|String|Yes|gp-xxxxxxxx|The endpoint that is used to connect to the specified database. |
|DBInstanceId|String|Yes|gp-xxxxxxxx|The ID of the instance. |
|Port|String|Yes|3432|The port number of the instance. Valid values: 3200 to 3999. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|ADD6EA90-EECB-4C12-9F26-0B6DB58710EF|The ID of the request. |

## Examples

Sample requests

```
http(s)://gpdb.aliyuncs.com/? Action=AllocateInstancePublicConnection
&ConnectionStringPrefix=gp-xxxxxxxx
&DBInstanceId=gp-xxxxxxxx
&Port=3432
&<Common request parameters>
```

Sample success responses

`XML` format

```
<AllocateInstancePublicConnectionResponse>  
       <RequestId>ADD6EA90-EECB-4C12-9F26-0B6DB58710EF</RequestId>
</AllocateInstancePublicConnectionResponse>
```

`JSON` format

```
{
   "RequestId": "ADD6EA90-EECB-4C12-9F26-0B6DB58710EF"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

