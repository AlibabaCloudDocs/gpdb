# AllocateInstancePublicConnection {#concept_ks2_hhm_q2b .concept}

## 描述 { .section}

分配实例外网地址的前缀。

## 输入参数 { .section}

|名称|类型|是否必须|描述|
|--|--|----|--|
|<公共请求参数\>|-|是|参见[公共参数](intl.zh-CN/API参考/公共参数.md#)。|
|Action|String|是|系统规定参数，取值：AllocateInstancePublicConnection。|
|DBInstanceId|String|是|实例名。|
|ConnectionStringPrefix|String|是|连接地址。|
|Port|String|是|端口号，范围为3200~3999。|

## 返回参数 { .section}

|名称|类型|描述|
|--|--|--|
|<公共返回参数\>|-|详见[公共返回参数](intl.zh-CN/API参考/公共参数.md#section_apd_1rv_3bb)。|

## 请求示例 {#section_mgb_1p5_42b .section}

```

https://gpdb.aliyuncs.com/?Action=AllocateInstancePublicConnection
&DBInstanceId=gp-xxxxxxx
&ConnectionStringPrefix=gp-xxxxxxx
&Port=3432
&<公共请求参数>
```

## 返回示例 { .section}

**XML格式**

```
<AllocateInstancePublicConnectionResponse>  
     <RequestId>ADD6EA90-EECB-4C12-9F26-0B6DB58710EF</RequestId>
</AllocateInstancePublicConnectionResponse>
```

**JSON格式**

```
{
   "RequestId": "ADD6EA90-EECB-4C12-9F26-0B6DB58710EF"
}
```

