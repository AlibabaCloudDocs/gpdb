# ST\_BuildPyramid

创建影像金字塔。

## 语法

```
raster ST_BuildPyramid(raster source,  
                       integer pyramidLevel default -1,  
                       ResampleAlgorithm algorithm default 'Near', 
                       cstring chunkTableName default '', 
                       cstring storageOption default '{}',
                       cstring buildOption default '{}');
```

## 参数

|参数名称|描述|
|:---|:-|
|source|需要创建金字塔的raster对象。|
|chunkTableName|金字塔所存储的分块表名称。只对基于对象存储（OSS）的栅格对象有效。|
|pyramidLevel|金字塔创建的层级， -1表示创建到最高层级。|
|algorithm|创建金字塔的重采样算法，取值如下：-   Near：最邻近
-   Average：平均值
-   Bilinear：二次线性
-   Cubic：三次卷积 |
|storageOption|JSON字符串，存储选项。描述raster对象金字塔的分块存储信息。该选项只针对基于对象存储OSS的栅格对象有效。|
|buildOption|JSON字符串，构建选项。当前支持参数parallel，可以设置操作并行度，数据类型为Integer，取值范围为1~64。不指定parallel时，使用GUC参数[ganos.parallel.degree]()的值。**说明：** 如果启用并行创建金字塔，则不支持事务。如果创建失败或需要对事务回滚，使用[ST\_deletePyramid](/intl.zh-CN/时空数据库/Raster SQL参考/金字塔操作/ST_deletePyramid.md)删除已经创建的金字塔。 |

storageOption参数说明如下。

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

