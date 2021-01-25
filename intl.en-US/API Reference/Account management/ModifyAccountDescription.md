# ModifyAccountDescription

## Description

Modifies the description of an account for an instance.

## Request parameters

|Parameter|Type|Required|Description|
|---------|----|--------|-----------|
|<Common request parameters\>|N/A|Yes|For more information, see [Common parameters](/intl.en-US/API Reference/Common parameters.md).|
|Action|String|Yes|The operation that you want to perform. Set the value to ModifyAccountDescription.|
|DBInstanceId|String|Yes|The ID of the instance.|
|AccountName|String|Yes|The name of the account. The account name must be unique and meet the following requirements:-   Starts with a letter.
-   Contains only lowercase letters, digits, or underscores \(\_\).
-   Be up to 16 characters in length.
-   Contains no reserved keywords. |
|AccountDescription|String|Yes|The new description of the account. The description must meet the following requirements:-   Starts with a letter.
-   Does not start with http:// or https://.
-   Contains letters, underscores \(\_\), hyphens \(-\), or digits.
-   Be 2 to 256 characters in length. |

## Response parameters

|Parameter|Type|Description|
|---------|----|-----------|
|<Common response parameters\>| |For more information, see [Common response parameters](/intl.en-US/API Reference/Common parameters.mdsection_apd_1rv_3bb).|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=ModifyAccountDescription
&DBInstanceId=gp-xxxxxxx
&AccountName=testAccout
&AccountDescription=testAccoutdescribe
&<Common request parameters>
```

## Sample responses

**XML format**

```
<ModifyAccountDescriptionResponse>
         <RequestId>99BBBD5E-B5D8-4FC8-B8BF-FB1A0A38BBA2</RequestId>
</ModifyAccountDescriptionResponse>
```

**JSON format**

```
{
    "RequestId": "99BBBD5E-B5D8-4FC8-B8BF-FB1A0A38BBA2"
}
```

