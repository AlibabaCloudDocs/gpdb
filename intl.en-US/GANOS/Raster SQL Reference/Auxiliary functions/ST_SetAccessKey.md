# ST\_SetAccessKey

Configures the logon information that is used to access a raster object stored in Alibaba Cloud Object Storage Service \(OSS\).

## Prerequisites

The raster object is stored in OSS.

## Syntax

```
raster ST_SetAccessKey(raster raster_obj, text id, text key, bool valid default true)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The name of the raster object.|
|id|The AccessKey ID that is used to log on to OSS. If you set this parameter to NULL, the previous AccessKey ID is used.|
|key|The AccessKey secret that is used to log on to OSS. If you set this parameter to NULL, the previous AccessKey secret is used.|
|valid|Specifies whether to verify the validity of the logon information.|

## Examples

```
UPDATE raster_table
SET rast = ST_SetAccessKey(rast, 'OSS_ACCESSKEY_ID', 'OSS_ACCESSKEY_SECRET');

SELECT ST_AKId(rast) from raster_table;
     st_akid      
------------------
 OSS_ACCESSKEY_ID
```

