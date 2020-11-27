# ST\_PixelHeight

获得raster对象在空间参考系下像素的高度。

## 前提条件

raster对象必须已经进行空间参考。

## 语法

```
 float8 ST_PixelHeight(raster raster_obj)
```

## 参数

|参数名称|描述|
|----|--|
|raster\_obj|raster对象。|

## 描述

PixelHeight如下图红色部分所示。如果raster的旋转为0，即等同于ScaleY。

![ST_PixelHeight](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/0229209951/p88912.png)

## 示例

```
select st_pixelwidth(rast), st_pixelheight(rast)
from raster_table;

 st_pixelwidth | st_pixelheight 
---------------+----------------
            60 |             60
```

