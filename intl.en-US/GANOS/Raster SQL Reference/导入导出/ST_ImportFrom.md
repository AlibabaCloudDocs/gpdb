# ST\_ImportFrom

This function imports an Object Storage Service \(OSS\) object to a raster object in AnalyticDB for PostgreSQL.

## Syntax

```
raster ST_ImportFrom(cstring chunkTableName, cstring url);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|chunkTableName|The name of the chunk table. The name must comply with the table naming conventions of AnalyticDB for PostgreSQL.|
|url|The URL of the imported OSS object. For more information, see the description of the url parameter in [ST\_CreateRast](/intl.en-US/GANOS/Raster SQL Reference/Raster creation/ST_CreateRast.md).|

## Description

The following table lists the supported raster formats.

|Format|Full name|
|------|---------|
|BMP|Microsoft Windows Device Independent Bitmap\(.bmp\)|
|EHdr|ESRI .hdr Labelled|
|ENVI|ENVI .hdr Labelled Raster|
|GTiff|TIFF/BigTIFF/GeoTIFF\(.tif\)|
|HFA|Erdas Imagine .img|
|RST|Idrisi Raster Format|
|INGR|Intergraph Raster Format|
|NetCDF|Network Common Data Form|
|AAIGrid|Arc/Info ASCII Grid|
|AIG|Arc/Info Binary Grid|
|GIF|Graphics Interchange Format|
|PNG|Portable Network Graphics \(.png\)|
|JPEG|JPEG JFIF File Format|

## Examples

```
Select ST_ImportFrom('chunk_table','OSS://<ak>:<ak_secret>@oss-cn-beijing-internal.aliyuncs.com/mybucket/data/image.tif');

-- Specify an OSS object that is in NetCDF and contains a subset.
Select ST_ImportFrom('chunk_table','OSS://<ak>:<ak_secret>@oss-cn-beijing-internal.aliyuncs.com/mybucket/data/image.nc:hcc');
```

