# Terms

This topic describes the terms related to Raster SQL.

|Term|Description|
|----|-----------|
|raster object|The regular grid that represents a space. Each cell in a raster object is assigned an attribute value to represent a data model of an entity. A raster object can be a satellite image, a digital elevation model \(DEM\), or a picture.|
|cell/pixel|The cell in a raster object, which is also called a pixel. The data type of cells can vary with the raster object, such as byte, short, integer, or double.|
|band|The measure of a single characteristic of a raster object. A band is represented by a single matrix of cell values. A raster object can have one or more bands.|
|chunk|The portion of a raster object. The size of a chunk can be customized, such as 256 × 256 × 3.|
|pyramid|The downsampled version of a raster object. A pyramid can contain one or more downsampled layers. Consecutive pyramid layers are downsampled at a scale of 2:1. Layer 0 stores the raw data.|
|pyramid level|The layer in a pyramid.|
|mosaic|The operation to integrate one or more raster objects into an existing raster dataset.|
|interleaving|The arrangement of pixels in a raster object. The interleaving types include band sequential \(BSQ\), band interleaved by pixel \(BIP\), and band interleaved by line \(BIL\).|
|world space|The geographic coordinates of a raster object.|
|raster space|The pixel coordinates of a raster object. The top-left corner of the raster object is used as the starting point.|
|metadata|The metadata of a raster object, which includes the spatial range, projection type, and pixel type. The metadata of the remote sensing platform is not included.|

