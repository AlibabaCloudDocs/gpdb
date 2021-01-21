# DeleteDatabase

You can call this operation to delete a database from an AnalyticDB for PostgreSQL instance.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=DeleteDatabase&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|DeleteDatabase|The operation that you want to perform. Set the value to DeleteDatabase. |
|DBInstanceId|String|Yes|gp-xxxxxxxxxxx|The ID of the instance. |
|DBName|String|No|testdb01|The name of the database. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|07F6177E-6DE4-408A-BB4F-0723301340F3|The ID of the request. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=DeleteDatabase
&DBName=testdb01
&DBInstanceId=gp-xxxxxxxxxxx
&<Common request parameters>
```

Sample success responses

`XML` format

```
<DeleteDatabaseResponse>
       <RequestId>07F6177E-6DE4-408A-BB4F-0723301340F3</RequestId>
</DeleteDatabaseResponse>
```

`JSON` format

```
{
    "RequestId":"07F6177E-6DE4-408A-BB4F-0723301340F3"
}
```

## Error code

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

