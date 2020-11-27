# ST\_PixelWidth

获得raster对象在空间参考系下像素的宽度。

## 语法

```
float8 ST_PixelWidth(raster raster_obj)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|

## 描述

PixelWidth如下图红色部分所示。如果raster的旋转为0，即等同于ScaleX。

![ST_PixelWidth](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/9129209951/p88911.png)

## 示例

```
select st_pixelwidth(rast), st_pixelheight(rast)
from raster_table;

 st_pixelwidth | st_pixelheight 
---------------+----------------
            60 |             60
```

