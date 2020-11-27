# ST\_Georeference

获得raster对象的地理参考信息。text格式的仿射参数为："A,B,C,D,E,F"。

## 语法

```
text ST_Georeference(raster raster_obj)
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|raster对象。|

## 示例

```
select ST_Georeference(raster_obj) from raster_table where id=1;

__________________________________
2.500000000000000,0.000000000000000,38604686.750000000000000,0.000000000000000,-2.500000000000000,4573895.750000000000000
```

