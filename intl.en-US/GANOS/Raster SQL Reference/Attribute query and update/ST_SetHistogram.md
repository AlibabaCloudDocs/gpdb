# ST\_SetHistogram

This function sets the histogram in JSON format for a specified band of a raster object.

## Syntax

```
raster ST_SetHistogram(raster rast, integer band, cstring histogram);
```

## Parameters

|Parameter|Description|
|---------|-----------|
|rast|A raster object.|
|band|The sequence number of the band, which starts from 0.|
|histogram|The histogram in JSON format.|

## Description

The following table describes the parameters of the histogram in JSON format.

|Parameter|Description|Type|Note|
|---------|-----------|----|----|
|approximate|Specifies whether to enable data sampling.|BOOLEAN|None|
|histsCounts|The array that displays the statistics.|INTEGER\[\]|None|
|binFunction/type|The type of the bin function.|STRING|-   linear
-   logarithm
-   explicit |
|binFunction/binTable/binValues|The bin values of the bin table.|NUMBER\[\]|This parameter takes effect only when the type of the bin function is explicit.|
|binFunction/binRange/minValue|The minimum value of the bin range.|NUMBER|This parameter takes effect only when the type of the bin function is logarithm or linear.|
|binFunction/binRange/maxValue|The maximum value of the bin range.|NUMBER|This parameter takes effect only when the type of the bin function is logarithm or linear.|
|binFunction/binRange/outRange|The value beyond the bin range.|NUMBER|This parameter takes effect only when the type of the bin function is logarithm or linear.|
|binFunction/binRange/binValues|The bin values of the bin range.|NUMBER\[\]|This parameter takes effect only when the type of the bin function is logarithm or linear.|

```
Example 1:
{
  "approximate":false,
  "histsCounts":
  [2,1,1,0,8,17,47,101,193,345,443,640,877,1189,1560,1847,2087,2560,2816,3193,3567,3840,4101,4415,4498,3876,3235,2458,1800,1598,1087,731,638,426,264,198,147,126,104,104,80,84,86,71,80,62,74,85,72,80,70,88,69,68,62,58,63,51,53,55,54,56,55,63,47,39,49,59,66,62,64,73,66,72,67,84,86,79,91,92,117,138,136,142,157,225,287,285,382,449,567,628,750,855,1021,1142,1242,1410,1504,1590,1786,1870,2044,2099,2277,2373,2451,2585,2646,2882,2878,3091,3396,3620,3911,4124,4304,4700,4893,5314,5446,5657,5765,5649,5749,5753,5601,5335,5161,4943,4592,4445,4207,4083,4090,4270,4465,4514,4844,5204,5331,5597,5777,5838,6004,6316,6095,5762,5567,5465,4923,4677,4220,3843,3401,3041,2571,2345,1972,1725,1376,1140,1008,841,716,548,442,373,308,212,133,78,68,31,24,12,2,2,1],
  "binFunction":
  {
    "type":"unknown",
    "binRange":
    {
      "minValue":29.0,
      "maxValue":208.0,
      "outRange":"include",
      "binValues":
      [29.0,30.0,31.0,32.0,33.0,34.0,35.0,36.0,37.0,38.0,39.0,40.0,41.0,42.0,43.0,44.0,45.0,46.0,47.0,48.0,49.0,50.0,51.0,52.0,53.0,54.0,55.0,56.0,57.0,58.0,59.0,60.0,61.0,62.0,63.0,64.0,65.0,66.0,67.0,68.0,69.0,70.0,71.0,72.0,73.0,74.0,75.0,76.0,77.0,78.0,79.0,80.0,81.0,82.0,83.0,84.0,85.0,86.0,87.0,88.0,89.0,90.0,91.0,92.0,93.0,94.0,95.0,96.0,97.0,98.0,99.0,100.0,101.0,102.0,103.0,104.0,105.0,106.0,107.0,108.0,109.0,110.0,111.0,112.0,113.0,114.0,115.0,116.0,117.0,118.0,119.0,120.0,121.0,122.0,123.0,124.0,125.0,126.0,127.0,128.0,129.0,130.0,131.0,132.0,133.0,134.0,135.0,136.0,137.0,138.0,139.0,140.0,141.0,142.0,143.0,144.0,145.0,146.0,147.0,148.0,149.0,150.0,151.0,152.0,153.0,154.0,155.0,156.0,157.0,158.0,159.0,160.0,161.0,162.0,163.0,164.0,165.0,166.0,167.0,168.0,169.0,170.0,171.0,172.0,173.0,174.0,175.0,176.0,177.0,178.0,179.0,180.0,181.0,182.0,183.0,184.0,185.0,186.0,187.0,188.0,189.0,190.0,191.0,192.0,193.0,194.0,195.0,196.0,197.0,198.0,199.0,200.0,201.0,202.0,203.0,204.0,205.0,206.0,207.0]
    }
  }
}

Example 2:
{
  "approximate" : true,
  "histsCounts" :[1,2,3,4,5],
  "binFunction" :
  {
    "type" : "explicit",
    "binTable" : 
    {
      "binValues" : [1.0,2.0,3.0,4.0,5.0]
    }
  }
}
```

## Example

```
UPDATE rat SET raster = st_sethistogram(raster, 0, '{"approximate":true,"histsCounts":[1,2,3,4,5],"binFunction":{"type":"explicit","binTable":{"binValues":[1.0,2.0,3.0,4.0,5.0]}}}') where id =1;

--------------------------------
(1 row)
```

