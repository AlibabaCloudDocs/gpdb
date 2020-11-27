# ST\_SetStatistics

设置raster对象的指定波段的统计值信息。

## 语法

```
raster ST_SetStatistics(raster rast, integer band, double min, double max, double mean, double std,cstring samplingParams);
```

## 参数

|参数名称|描述|
|----|--|
|rast|raster对象。|
|band|指定的波段序号，从0开始。|
|min,max,mean,std|统计值。|
|samplingParams|\{"approx":false\}或者 \{"approx":true\}。|

## 示例

```
update rast set rast=ST_SetStatistics(rast,0,0.0 , 255.0, 125.0, 23.6, '{"approx":false}');

__________________________________
(1 row)
```

