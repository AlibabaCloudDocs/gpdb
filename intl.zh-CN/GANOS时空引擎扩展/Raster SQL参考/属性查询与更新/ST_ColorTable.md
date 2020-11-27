# ST\_ColorTable

获取raster对象的某一个波段的颜色表信息，返回颜色表的JSON格式。

## 语法

```
text ST_ColorTable(raster raster_obj, integer band);
```

## 参数

|参数名称|描述|
|:---|:-|
|raster\_obj|raster对象。|
|band|指定的波段序号，从0开始。|

## 描述

颜色表的JSON格式：

-   4分量：

    ```
    '{"compsCount":4,
        "entries":[
            {"value":0,"c1":0,"c2":0,"c3":0,"c4":255},
            {"value":1,"c1":0,"c2":0,"c3":85,"c4":255},
            {"value":2,"c1":0,"c2":0,"c3":170,"c4":255}
        ]
    }'
    ```

-   3分量：

    ```
    '{"compsCount":3,
        "entries":[
            {"value":0,"c1":0,"c2":0,"c3":0},
            {"value":1,"c1":0,"c2":0,"c3":85},
            {"value":2,"c1":0,"c2":0,"c3":170}
        ]
    }'
    ```


如果不存在颜色表，函数返回空值。

## 示例

```
select ST_ColorTable(raster_obj,0) from raster_table where id = 1;

__________________________________
'{"compsCount":3,
    "entries":
    [
        {"value":0,"c1":0,"c2":0,"c3":0},
        {"value":1,"c1":0,"c2":0,"c3":85},
        {"value":2,"c1":0,"c2":0,"c3":170}
    ]
}'
```

