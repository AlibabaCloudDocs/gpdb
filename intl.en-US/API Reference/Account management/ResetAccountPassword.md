# ResetAccountPassword

## Description

Resets the password of an account for an instance. Before you call this operation, make sure that the following requirements are met:

-   The instance is in the running state.
-   The instance is not locked.

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to ResetAccountPassword.|
|DBInstanceId|String|Yes|The ID of the instance.|
|AccountName|String|Yes|The name of the account.|
|AccountPassword|String|Yes|The new password for the account. The password must be 8 to 32 characters in length and contain at least three of the following character types: uppercase letters, lowercase letters, digits, and special characters. Special characters include: ! @ \# $ % ^ & \* \( \) \_ + - =|

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>| |For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=ResetAccountPassword
&AccountName=testaccount_1
&AccountPassword=Testaccount_1
&DBInstanceId=gp-xxxxxxx
&<Common request parameters>
```

## Sample responses

**XML format**

```
<ResetAccountPasswordResponse>
         <RequestId>187C80FC-75C4-477C-BBF2-A368A36D041C</RequestId>
</ResetAccountPasswordResponse>
```

**JSON format**

```
{
    "RequestId":"187C80FC-75C4-477C-BBF2-A368A36D041C"
}
```

