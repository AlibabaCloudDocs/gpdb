# ST\_ExportTo

将一个raster对象导出为OSS文件。

## 语法

```
boolean ST_ExportTo(raster source,  cstring format, cstring url,  integer level = 0);
```

## 参数

|参数名称|描述|
|:---|:-|
|source|需要导出的raster对象。|
|format|导出的数据，常见如 GTiff，BMP 等。|
|url|外部文件路径。详情请参见[ST\_CreateRast](/cn.zh-CN/GANOS时空引擎扩展/Raster SQL参考/R​aster创建/ST_CreateRast.md) 中构建路径的描述。|
|level|金字塔级别。|

## 描述

导出成功返回true，失败则返回false。

format指定导出格式的名称，常见格式如下。

|名称|全称|
|--|--|
|BMP|Microsoft Windows Device Independent Bitmap\(.bmp\)|
|EHdr|ESRI .hdr Labelled|
|ENVI|ENVI .hdr Labelled Raster|
|GTiff|TIFF/BigTIFF/GeoTIFF\(.tif\)|
|HFA|Erdas Imagine .img|
|RST|Idrisi Raster Format|
|INGR|Intergraph Raster Format|

## 示例

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

