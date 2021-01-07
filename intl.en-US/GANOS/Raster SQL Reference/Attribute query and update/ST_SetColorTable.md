# ST\_SetColorTable

This function sets the color table in JSON format for a specified band of a raster object.

## Syntax

```
raster ST_SetColorTable(raster rast, integer band_sn, cstring clb);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|rast|The raster object.|
|band\_sn|The sequence number of the band, which starts from 0.|
|clb|The color table in JSON format. For more information, see [ST\_ColorTable](/intl.en-US/Spatio-temporal Database/Raster SQL reference/Attribute query and update/ST_ColorTable.md).|

## Examples

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

