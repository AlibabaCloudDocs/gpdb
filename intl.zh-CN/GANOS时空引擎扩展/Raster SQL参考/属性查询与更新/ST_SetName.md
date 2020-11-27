# ST\_SetName

设置raster对象的名称。

## 语法

```
raster ST_SetName(raster rast, cstring name);
```

## 参数

|参数名称|描述|
|----|--|
|rast|raster对象。|
|name|新的对象名称。|

## 示例

```
update rat set rast = ST_SetName(rast,'image2') where id = 2;

——————————————————————————————————
(1 row)
```

