# ST\_ExportTo

This function exports a raster object as an Object Storage Service \(OSS\) object.

## Syntax

```
boolean ST_ExportTo(raster source, cstring format, cstring url, integer level = 0);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|source|The raster object to be exported.|
|format|The format of the exported data, such as GTiff or BMP.|
|url|The URL of the exported OSS object. For more information, see the description of the url parameter in [ST\_CreateRast](/intl.en-US/GANOS/Raster SQL Reference/Raster creation/ST_CreateRast.md).|
|level|The pyramid level.|

## Description

If the raster object is successfully exported, the function returns true. If the raster object fails to be exported, the function returns false.

The format parameter specifies the format of the exported data. The following table lists the common raster formats.

|Format|Full name|
|------|---------|
|BMP|Microsoft Windows Device Independent Bitmap\(.bmp\)|
|EHdr|ESRI .hdr Labelled|
|ENVI|ENVI .hdr Labelled Raster|
|GTiff|TIFF/BigTIFF/GeoTIFF\(.tif\)|
|HFA|Erdas Imagine .img|
|RST|Idrisi Raster Format|
|INGR|Intergraph Raster Format|

## Examples

```
DO $$
declare
    rast raster;
begin
    select raster_obj into rast from raster_table where id = 1;
    Select ST_ExportTo(rast, 'GTiff', 'OSS://ABCDEFG:1234567890@oss-cn-beijing-internal.aliyuncs.com/mybucket/data/4.tif');
end;    
$$ LANGUAGE 'plpgsql';
```

