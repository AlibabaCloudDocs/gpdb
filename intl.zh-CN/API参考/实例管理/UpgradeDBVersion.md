# UpgradeDBVersion

## 描述

调用UpgradeDBVersion为指定实例升级内核小版本。

## 请求参数

|名称|类型|是否必选|示例值|描述|
|--|--|----|---|--|
|Action|String|是|UpgradeDBVersion|系统规定参数。取值：UpgradeDBVersion。|
|DBInstanceId|String|是|gp-xxxxxxxxxxxxx|实例ID。|

## 返回参数

|名称|类型|示例值|描述|
|--|--|---|--|
|RequestId|String|25C11EE5-B7E8-481A-A07C-BD619971A570|请求ID。|
|DBInstanceId|String|gp-xxxxxxxxxxxxx|实例ID。|
|DBInstanceName|String|gp-xxxxxxxxxxxxx|实例名称。|
|TaskId|String|101450956|任务ID。|

## 请求示例

```
https://gpdb.aliyuncs.com/?Action=UpgradeDBVersion
&DBInstanceId=gp-wz9kmr708m155j***
&<公共请求参数>
```

## 返回示例

```
{
  "requestId": "25C11EE5-B7E8-481A-A07C-BD619971A570",
  "DBInstanceId": "gp-wz9kmr708m155j***",
  "DBInstanceName": "gp-wz9kmr708m155j***",
  "TaskId": "101450956"
}
```

