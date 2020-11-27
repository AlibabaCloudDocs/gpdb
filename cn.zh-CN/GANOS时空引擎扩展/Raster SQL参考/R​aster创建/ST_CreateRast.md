# ST\_CreateRast

创建一个基于阿里云对象存储服务（OSS）的raster对象。

## 语法

```
raster ST_CreateRast(cstring url);
raster ST_CreateRast(cstring url, cstring storageOption);
```

## 参数

|参数名称|描述|
|:---|:-|
|url|OSS影像文件的路径。如果是具有SubSet的NetCDF，可以通过`:<name>`方式指定。|
|storageOption|基于JSON格式的字符串，描述raster对象金字塔的分块存储信息。|

storageOption支持的参数如下：

|参数名称|描述|类型|格式|默认值|说明|
|----|--|--|--|---|--|
|chunkdim|分块的维度信息|string|\(w, h, b\)|从原始影像中读取分块大小|无|
|interleaving|交错方式|string|无|bsq|必须是以下一种：-   bip：像素交错
-   bil：行交错
-   bsq：波段交错
-   auto：根据原始影像自动选择 |

**说明：** 在通常情况下无需修改分块和交错信息，仅在特殊场景下需要修改，例如：

-   需要多波段RGB组合浏览，但是默认值为bsq（波段交错） ，需要修改为bip（像素交错）。
-   某些影像的分块大小为1行\*n列，但是访问请求为256行\*256列的规则块，需要修改为按照访问请求的大小。

## 描述

OSS文件路径格式如下： `oss://access_id:secrect_key@Endpoint/path_to/file`。 其中Endpoint可以被省略，系统会自动寻找相应的Endpoint。如果Endpoint被省略，路径必须以/开头。

Endpoint为OSS的地域节点。为保证数据导入的性能，请确保ADB PG与OSS所在地域相同，详情请参见[OSS Endpoint](https://help.aliyun.com/document_detail/31834.html)。

## 示例

```
-- 基于OSS存储，指定accessID,accessKEY,endpoint
Select ST_CreateRast('OSS://<ak>:<ak_secret>@oss-cn-beijing-internal.aliyuncs.com/mybucket/data/image.tif');

-- 指定分块与交错方式
Select ST_CreateRast('OSS://<ak>:<ak_secret>@oss-cn-beijing-internal.aliyuncs.com/mybucket/data/image.tif', '{"chunkdim":"(256,256,3)","interleaving":"auto"}');

-- 指定具有Subset的NetCDF对应的影像
Select ST_CreateRast('OSS://<ak>:<ak_secret>@oss-cn-beijing-internal.aliyuncs.com/mybucket/data/image.nc:hcc');
```

