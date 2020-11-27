# ST\_ConvexHull

根据栅格的地理参考信息获得栅格对象的凸包。

## 语法

```
geometry ST_ConvexHull(raster source);
geometry ST_ConvexHull(raster source, 
                       integer pyramid);
```

## 参数

|参数名称|描述|
|:---|:-|
|source|需要计算的raster对象。|
|pyramid|金字塔层级，从0开始，默认值为0。|

## 描述

如下图所示，红色外框包含的部分为栅格对象的凸包。

![凸包](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/zh-CN/1613129951/p129345.png)

## 示例

```
SELECT st_astext(st_convexhull(rast_object)）
FROM raster_table;

----------------------------------------------------
 POLYGON((-180 90,180 90,180 -90,-180 -90,-180 90))

-- 指定金字塔层级                 
SELECT st_astext(st_convexhull(rast_object,  1)）
FROM raster_table;

----------------------------------------------------
 POLYGON((-180 90,180 90,180 -90,-180 -90,-180 90)) 
```

