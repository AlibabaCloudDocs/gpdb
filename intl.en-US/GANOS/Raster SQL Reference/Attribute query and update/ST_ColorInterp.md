# ST\_ColorInterp

This function returns the color interpretation type of a band of a raster object.

## Syntax

```
text ST_ColorInterp(raster raster_obj, integer band);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|raster\_obj|The raster object.|
|band|The sequence number of the band, which starts from 0.|

The following table describes the values of the interp parameter that is returned.

|Value|Description|
|-----|-----------|
|Undefined|The color interpretation type is not defined.|
|GrayIndex|The index of the associated gray value table.|
|RGBIndex|The index of the RGB color table.|
|RGBAIndex|The index of the RGBA color table.|
|RedBand|The red band in the RGB color model.|
|GreenBand|The green band in the RGB color model.|
|BlueBand|The blue band in the RGB color model.|
|AlphaBand|The alpha band in the RGBA color model.|
|HueBand|The hue band in the HSL color model.|
|SaturationBand|The saturation band in the HSL color model.|
|LightnessBand|The lightness band in the HSL color model.|
|CyanBand|The cyan band in the CMYK color model.|
|MagentaBand|The magenta band in the CMYK color model.|
|YellowBand|The yellow band in the CMYK color model.|
|BlackBand|The black band in the CMYK color model.|
|YCbCr\_YBand|The Y band in the YCbCr color model.|
|YCbCr\_CbBand|The Cb band in the YCbCr color model.|
|YCbCr\_CrBand|The Cr band in the YCbCr color model.|

## Example

```
select ST_ColorInterp(raster_obj,0) from raster_table where id = 1;

__________________________________
RedBand
```

