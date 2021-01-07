# ST\_SetColorInterp

This function sets the color interpretation type for a specified band of a raster object.

## Syntax

```
raster ST_SetColorInterp(raster rast, integer band_sn, ColorInterp interp)
```

## Parameters

|Parameter|Description|
|---------|-----------|
|rast|The raster object.|
|band\_sn|The sequence number of the band, which starts from 0.|
|interp|The color interpretation type.|

## Description

The following table describes the values of the interp parameter.

|Value|Description|
|-----|-----------|
|Undefined|The color interpretation type is not defined.|
|GrayIndex|The index of the associated gray color table.|
|RGBIndex|The index of the associated RGB color table.|
|RGBAIndex|The index of the associated RGBA color table.|
|CMYKIndex|The index of the associated CMYK color table.|
|HSLIndex|The index of the associated HSL color table.|
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
|YBand|The Y band in the YCbCr color model.|
|CbBand|The Cb band in the YCbCr color model.|
|CrBand|The Cr band in the YCbCr color model.|

## Examples

```
update rast set rast=ST_SetColorInterp(rast,0, 'CI_Cyan');

__________________________________
(1 row)
```

