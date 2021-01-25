# ST\_ColorTable

This function returns the color table of a band of a raster object in JSON format.

## Syntax

```
text ST_ColorTable(raster raster_obj, integer band);
```

## Parameters

|Parameter|Description|
|:--------|:----------|
|raster\_obj|The raster object.|
|band|The sequence number of the band, which starts from 0.|

## Description

The following code shows color tables in JSON format.

-   Four color components:

    ```
    '{"compsCount":4,
        "entries":[
            {"value":0,"c1":0,"c2":0,"c3":0,"c4":255},
            {"value":1,"c1":0,"c2":0,"c3":85,"c4":255},
            {"value":2,"c1":0,"c2":0,"c3":170,"c4":255}
        ]
    }'
    ```

-   Three color components:

    ```
    '{"compsCount":3,
        "entries":[
            {"value":0,"c1":0,"c2":0,"c3":0},
            {"value":1,"c1":0,"c2":0,"c3":85},
            {"value":2,"c1":0,"c2":0,"c3":170}
        ]
    }'
    ```


If the band does not have a color table, the function returns null.

## Examples

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

