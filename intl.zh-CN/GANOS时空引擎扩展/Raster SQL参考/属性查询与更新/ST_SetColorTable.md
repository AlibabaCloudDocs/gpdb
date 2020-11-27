# ST\_SetColorTable

设置raster对象的指定波段的颜色表信息，采用颜色表的JSON格式。

## 语法

```
raster ST_SetColorTable(raster rast, integer band_sn, cstring clb);
```

## 参数

|参数名称|描述|
|----|--|
|rast|raster对象。|
|band\_sn|指定的波段序号，从0开始。|
|clb|颜色表的JSON格式，参见[ST\_ColorTable](/intl.zh-CN/时空数据库/Raster SQL参考/属性的查询与更新/ST_ColorTable.md)|

## 示例

```
update rast set rast=ST_SetColorTable(rast,0, 
'{"compsCount":4,
    "entries":[
        {"value":0,"c1":0,"c2":0,"c3":0,"c4":255},
        {"value":1,"c1":0,"c2":0,"c3":85,"c4":255},
        {"value":2,"c1":0,"c2":0,"c3":170,"c4":255}
    ]
}');

__________________________________
(1 row)
```

