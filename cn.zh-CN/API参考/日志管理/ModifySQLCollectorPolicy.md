# ModifySQLCollectorPolicy {#concept_wmq_fhm_q2b .concept}

## 描述 { .section}

开启或关闭指定实例的SQL采集功能。

## 请求参数 { .section}

|名称|类型|是否必须|描述|
|--|--|----|--|
|<公共请求参数\>|-|是|参见[公共参数](cn.zh-CN/API参考/公共参数.md#)。|
|Action|String|是|系统规定参数，取值：ModifySQLCollectorPolicy。|
|DBInstanceId|String|是|实例名。|
|SQLCollectorStatus|String|是| -   Enable：SQL采集开启
-   Disabled：SQL采集关闭

 |

## 返回参数 { .section}

|名称|类型|描述|
|--|--|--|
|<公共返回参数\>|-|详见[公共参数](https://help.aliyun.com/document_detail/26224.html)。|

## 请求示例 { .section}

```
https://gpdb.aliyuncs.com/?Action=ModifySQLCollectorPolicy
&DBInstanceId=gp-bp1gjo105888f3b69
&SQLCollectorStatus=Enable
&<公共请求参数>
```

## 返回示例 { .section}

**XML格式**

```
<ModifySQLCollectorPolicyResponse>
         <RequestId>4FA1F1D1-50A6-4F60-9A78-5752F2076A53</RequestId>
</ModifySQLCollectorPolicyResponse>

```

**JSON格式**

```
{
  "RequestId":"4FA1F1D1-50A6-4F60-9A78-5752F2076A53"
}
```

