# ST\_UnGeoreference

去掉raster对象的地理参考信息。

## 语法

```
 raster ST_UnGeoreference(raster rast)
```

## 参数

|参数名称|描述|
|----|--|
|rast|raster对象。|

## 示例

```
update rast set rast=ST_UnGeoreference(rast) where id=1;

__________________________________
(1 row)
```

