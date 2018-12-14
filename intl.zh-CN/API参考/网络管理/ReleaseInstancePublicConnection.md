# ReleaseInstancePublicConnection {#concept_tmj_qhm_q2b .concept}

## 描述 { .section}

该接口用于释放实例的外网连接。

## 请求参数 { .section}

|名称|类型|是否必须|描述|
|--|--|----|--|
|<公共请求参数\>|-|是|参见[公共参数](intl.zh-CN/API参考/公共参数.md#)。|
|Action|String|是|系统规定参数，取值为ReleaseInstancePublicConnection。|
|DBInstanceId|String|是|实例名。|
|CurrentConnectionString|String|是|外网连接。|

## 返回参数 { .section}

|名称|类型|描述|
|--|--|--|
|<公共返回参数\>|-|详见[公共返回参数](intl.zh-CN/API参考/公共参数.md#section_apd_1rv_3bb)。|

## 请求示例 { .section}

```
https://gpdb.aliyuncs.com/?Action=ReleaseInstancePublicConnection
&DBInstanceId=gp-xxxxxxx
&CurrentConnectionString=gp-xxxxxxx.gpdb.rds.aliyuncs.com
&<公共请求参数>
```

## 返回示例 { .section}

**XML格式**

```
<ReleaseInstancePublicConnectionResponse>  
         <RequestId>9CEF7037-4158-4A65-BEC7-2A1541F6FE74</RequestId>
</ReleaseInstancePublicConnectionResponse>
```

**JSON格式**

```
{
   "RequestId": "9CEF7037-4158-4A65-BEC7-2A1541F6FE74"
}
```

