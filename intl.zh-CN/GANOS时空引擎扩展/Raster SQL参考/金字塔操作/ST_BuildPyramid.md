# ST\_BuildPyramid

创建影像金字塔。

## 语法

```
raster ST_BuildPyramid(raster source);
raster ST_BuildPyramid(raster source,  cstring chunkTableName);
raster ST_BuildPyramid(raster source,  integer pyramidLevel, ResampleAlgorithm algorithm,
                       cstring chunkTableName, cstring storageOption);
```

## 参数

|参数名称|描述|
|:---|:-|
|source|需要创建金字塔的raster对象。|
|chunkTableName|金字塔所存储的分块表名称。|
|pyramidLevel|金字塔创建的层级， -1表示创建到最高层级。|
|algorithm|创建金字塔的重采样算法，取值如下：-   Near：最邻近
-   Average：平均值
-   Bilinear：二次线性
-   Cubic：三次卷积 |
|storageOption|存储选项。该选项只针对基于对象存储OSS的栅格对象有效。|

storageOption是基于JSON格式的字符串，描述raster对象金字塔的分块存储信息。支持的参数如下。

|参数名称|类型|说明|
|----|--|--|
|chunkdim|string|分块的维度信息，格式为`(w, h, b)`，默认从原始影像中读取分块大小。|
|interleaving|string|交错方式，取值如下：-   bip：像素交错
-   bil：行交错
-   bsq（默认值）：波段交错
-   auto：根据原始影像自动选择 |
|compression|string|压缩算法类型，取值如下：-   none
-   jpeg
-   zlib
-   png
-   lzo
-   lz4（默认值）
-   zstd
-   snappy
-   jp2k |
|quality|integer|压缩质量。只针对jpeg和jp2k压缩算法生效，默认值为75。|

## 描述

创建金字塔支持GPU加速，如果运行环境带有GPU设备，则Ganos会自动开启GPU加速功能。

## 示例

```
DO $$
declare
    rast raster;
begin
    select raster_obj into rast from raster_table where id = 1;
    rast = st_buildpyramid(rast);
    update raster_table set raster_obj = rast where id = 1;
end;    
$$ LANGUAGE 'plpgsql';

DO $$
declare
    rast raster;
begin
    select raster_obj into rast from raster_table where id = 1;
    rast = st_buildpyramid(rast, 'chunk_table');
    update raster_table set raster_obj = rast where id = 1;
end;    
$$ LANGUAGE 'plpgsql';
```

