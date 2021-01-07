# ST\_CreateRast

This function creates a raster object based on Object Storage Service \(OSS\).

## Syntax

```
raster ST_CreateRast(cstring url);
raster ST_CreateRast(cstring url, cstring storageOption);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|url|The path of the OSS object based on which you want to create a raster object. If the OSS object is in Network Common Data Form \(NetCDF\) and contains a subset, you can specify the subset in the `:<name>` format.|
|storageOption|A JSON string that describes the chunks for storing the pyramid of the raster object.|

The following table describes fields in the storageOption parameter.

|Field|Description|Type|Format|Default value|Setting note|
|-----|-----------|----|------|-------------|------------|
|chunkdim|The size of each chunk that is used to store the data of the raster object.|string|\(w, h, b\)|Same as the size of each chunk in the OSS object|None.|
|interleaving|The interleaving type of the new raster object.|string|None.|bsq|Valid values:-   bip: band interleaved by pixel \(BIP\)
-   bil: band interleaved by line \(BIL\)
-   bsq: band sequential \(BSQ\)
-   auto: a value that is specified by the system based on the OSS object |

**Note:** You need to change the default values of the chunkdim and interleaving fields only in some cases:

-   Users want to view the raster object based on multiband \(red, green, and blue\) RGB combination, but the value of the interleaving field is bsq. In this case, change the value of the interleaving field to bip.
-   The chunks of some images that are used to render the raster object contain 1 row and n columns in size. However, users request chunks that contain 256 rows and 256 columns in size. In this case, you must change the value of the chunkdim field to the requested chunk size.

## Description

You can specify the path of the OSS object in the following format: `oss://access_id:secrect_key@Endpoint/path_to/file`. The endpoint is optional. If you do not specify the endpoint, the system finds the endpoint and you must make sure that the path starts with a forward slash \(/\).

The endpoint is the domain name that you can use to access the OSS bucket where the OSS object is stored. For the best import performance, make sure that the AnalyticDB for PostgreSQL instance resides in the same region as the OSS bucket. For more information, see[OSS Endpoint](https://www.alibabacloud.com/help/zh/doc-detail/31834.htm).

## Examples

```
-- Specify the AccessKey ID, AccessKey secret, and endpoint in the URL of an OSS object to create a raster object.
Select ST_CreateRast('OSS://<ak>:<ak_secret>@oss-cn-beijing-internal.aliyuncs.com/mybucket/data/image.tif');

-- Specify the chunk size and the interleaving type of a raster object.
Select ST_CreateRast('OSS://<ak>:<ak_secret>@oss-cn-beijing-internal.aliyuncs.com/mybucket/data/image.tif', '{"chunkdim":"(256,256,3)","interleaving":"auto"}');

-- Specify an OSS object that is in NetCDF and contains a subset.
Select ST_CreateRast('OSS://<ak>:<ak_secret>@oss-cn-beijing-internal.aliyuncs.com/mybucket/data/image.nc:hcc');
```

