# ModifySecurityIps {#concept_lb2_vgm_q2b .concept}

## 描述 { .section}

修改允许访问实例的IP名单。接口调用必须满足以下条件，否则将失败：

-   实例运行中
-   实例没有被锁定

## 输入参数 { .section}

|名称|类型|是否必须|描述|
|--|--|----|--|
|<公共请求参数\>|-|是|参见[公共参数](intl.zh-CN/API参考/公共参数.md#)。|
|Action|String|是|系统规定参数，取值：ModifySecurityIps。|
|DBInstanceId|String|是|实例名。|
|SecurityIPList|String|是|IP白名单分组下的IP列表，最多1000个，以逗号隔开，格式如下：-   0.0.0.0/0；
-   10.23.12.24（IP）；
-   10.23.12.24/24（CIDR模式，无类域间路由，/24表示地址中前缀的长度，范围为\[1,32\]）。

|
|DBInstanceIPArrayName|String|否|IP白名单分组的名字，如果不传默认操作“Default”分组。**说明：** 1个实例最多支持50个白名单分组。

|
|DBInstanceIPArrayAttribute|String|否|默认为空。用于区分不同的属性值，控制台不显示带有`hidden`属性的分组。|

## 返回参数 { .section}

|名称|类型|描述|
|--|--|--|
|<公共返回参数\>|-|详见[公共返回参数](intl.zh-CN/API参考/公共参数.md#section_apd_1rv_3bb)。|

## 请求示例 { .section}

```
https://gpdb.aliyuncs.com/?Action=ModifySecurityIps
&DBInstanceId=gp-xxxxxxx
&SecurityIPList=192.168.0.1
&<公共请求参数>
```

## 返回示例 { .section}

**XML格式**

```
<ModifySecurityIpsResponse>
         <RequestId>871C698F-B43D-4D1D-ACD6-DF56B0F89978</RequestId>
</ModifySecurityIpsResponse>
```

**JSON格式**

```
{
  "RequestId":"871C698F-B43D-4D1D-ACD6-DF56B0F89978"
}
```

