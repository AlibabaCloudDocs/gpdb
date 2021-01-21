# UpgradeDBVersion

## Description

You can call this operation to upgrade the minor kernel version of a specified instance.

## Request parameters

|Parameter|Type|Required|Example|Description|
|---------|----|--------|-------|-----------|
|Action|String|Yes|UpgradeDBVersion|The operation that you want to perform. Set the value to UpgradeDBVersion.|
|DBInstanceId|String|Yes|gp-xxxxxxxxxxxxx|The ID of the instance.|

## Response parameters

|Parameter|Type|Example|Description|
|---------|----|-------|-----------|
|RequestId|String|25C11EE5-B7E8-481A-A07C-BD619971A570|The ID of the request.|
|DBInstanceId|String|gp-xxxxxxxxxxxxx|The ID of the instance.|
|DBInstanceName|String|gp-xxxxxxxxxxxxx|The name of the instance.|
|TaskId|String|101450956|The ID of the task.|

## Sample requests

```
https://gpdb.aliyuncs.com/?Action=UpgradeDBVersion
&DBInstanceId=gp-wz9kmr708m155j***
&<Common request parameters>
```

## Sample responses

```
{
  "requestId": "25C11EE5-B7E8-481A-A07C-BD619971A570",
  "DBInstanceId": "gp-wz9kmr708m155j***",
  "DBInstanceName": "gp-wz9kmr708m155j***",
  "TaskId": "101450956"
}
```

