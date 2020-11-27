# ST\_ImportFrom

从一个OSS文件导入到数据库。

## 语法

```
raster ST_ImportFrom(cstring chunkTableName, cstring url);
```

## 参数

|参数名称|描述|
|:---|:-|
|chunkTableName|块表的名称，名称必须符合数据库表名的规范。|
|u​rl|外部文件路径。详情请请参见[ST\_CreateRast](/cn.zh-CN/GANOS时空引擎扩展/Raster SQL参考/R​aster创建/ST_CreateRast.md)中构建路径的描述。|

## 描述

支持导入的数据类型如下：

|名称|全称|
|--|--|
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

## 示例

```
Select ST_ImportFrom('chunk_table','OSS://<ak>:<ak_secret>@oss-cn-beijing-internal.aliyuncs.com/mybucket/data/image.tif');

-- 指定具有Subset的NetCDF对应的影像
Select ST_ImportFrom('chunk_table','OSS://<ak>:<ak_secret>@oss-cn-beijing-internal.aliyuncs.com/mybucket/data/image.nc:hcc');
```

