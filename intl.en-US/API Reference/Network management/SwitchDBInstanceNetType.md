# SwitchDBInstanceNetType

You can call this operation to switch between internal and public endpoints.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=SwitchDBInstanceNetType&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|SwitchDBInstanceNetType|The operation that you want to perform. Set the value to SwitchDBInstanceNetType. |
|ConnectionStringPrefix|String|Yes|test1234|The prefix of the custom endpoint.

 -   The prefix can contain lowercase letters, digits, and hyphens \(-\) and must start with a lowercase letter.
-   The prefix can be up to 30 characters in length. |
|DBInstanceId|String|Yes|rm-uf6wjk5xxxxxxx|The ID of the instance. |
|Port|String|Yes|3306|The number of the port that is used to connect to the instance. Valid values: 3001 to 3999. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|FA67B751-2A2D-470C-850B-D6B93699D35C|The ID of the request. |

## Examples

Sample requests

```
http(s)://[Endpoint]/?Action=SwitchDBInstanceNetType
&ConnectionStringPrefix=test1234
&DBInstanceId=rm-uf6wjk5xxxxxxx
&Port=3306
&<Common request parameters>
```

Sample success responses

`XML` format

```
<RequestId>FA67B751-2A2D-470C-850B-D6B93699D35C</RequestId>
```

`JSON` format

```
{"RequestId":"FA67B751-2A2D-470C-850B-D6B93699D35C"}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

