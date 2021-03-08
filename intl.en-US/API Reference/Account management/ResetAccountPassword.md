# ResetAccountPassword

You can call this operation to reset the password of a specified account for an AnalyticDB for PostgreSQL instance.

Before you call this operation, make sure that the following requirements are met:

-   The instance is in the running state.
-   The instance is not locked.

## Debugging

[OpenAPI Explorer automatically calculates the signature value. For your convenience, we recommend that you call this operation in OpenAPI Explorer. OpenAPI Explorer dynamically generates the sample code of the operation for different SDKs.](https://api.aliyun.com/#product=gpdb&api=ResetAccountPassword&type=RPC&version=2016-05-03)

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|AccountName|String|Yes|testaccount\_1|The name of the account. |
|AccountPassword|String|Yes|Testaccount\_1|The new password for the account. The password must be 8 to 32 characters in length and contain at least three of the following character types: uppercase letters, lowercase letters, digits, and special characters. Special characters include `! @ # $ % ^ & * ( ) _ + - =` |
|DBInstanceId|String|Yes|gp-xxxxxxxxx|The ID of the instance. |

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|187C80FC-75C4-477C-BBF2-A368A36D041C|The ID of the request. |

## Examples

Sample requests

```
https://gpdb.aliyuncs.com/?Action=ResetAccountPassword
&AccountName=testaccount_1
&AccountPassword=Testaccount_1
&DBInstanceId=gp-xxxxxxxxx
&<Common request parameters>
```

Sample success responses

`XML` format

```
<ResetAccountPasswordResponse>
           <RequestId>187C80FC-75C4-477C-BBF2-A368A36D041C</RequestId>
</ResetAccountPasswordResponse>
```

`JSON` format

```
{
    "RequestId":"187C80FC-75C4-477C-BBF2-A368A36D041C"
}
```

## Error codes

For a list of error codes, visit the [API Error Center](https://error-center.alibabacloud.com/status/product/gpdb).

