# Load vector data

This topic describes how to load a vector data file into Ganos by using shp2pgsql, ogr2ogr, or QGIS.

## Preparations

Before you load a vector data file, run the following command on your AnalyticDB for PostgreSQL instance to create the ganos\_geometry extension:

```
CREATE EXTENSION ganos_geometry CASCADE
```

## Load vector data by using shp2pgsql

The shp2pgsql command line tool is used to convert Esri shapefiles into SQL files. For more information, visit [shp2pgsql](https://postgis.net/docs/using_postgis_dbmanagement.html#idm2274). Then, you can load the SQL files into Ganos.

-   Syntax

    ```
    shp2pgsql -s <srid> -c -W <charset> <path_to_shpfile> <schema>.<table_name> | psql -d <dbname> -h <host> -U <user_name> -p <port>
    ```

-   Parameters

    |Parameter|Description|Example|
    |---------|-----------|-------|
    |-s <srid\>|The spatial reference system identifier \(SRID\) of the vector data file.|4490|
    |-c|Specifies to create a table.You can also use the following parameters:

    -   -a: appends data to a table.
    -   -d: deletes a table before data is loaded.
    -   -p: creates a schema in SQL without writing spatial data.
|None|
    |-W <charset\>|The character set that is used by the shapefile.|GBK or UTF8|
    |<path\_to\_shpfile\>|The storage path of the shapefile. The storage path must be accessible to the shp2pgsql command line tool.|/home/roads.shp|
    |<schema\>.<table\_name\>|The name of the geometry table that you created.|public.roads|
    |-d <dbname\>|The name of the database.|mydb|
    |-h <host\>|The endpoint of the instance on which to create the database.|pgm-xxxxxxx.pg.rds.aliyuncs.com|
    |-U <user\_name\>|The username that is used to connect to the database.|my\_name|
    |-p <port\>|The port number that is used to connect to the database.|3433|

-   Example

    ```
    shp2pgsql -s 4490 -c -W "GBK" /home/roads.shp public.roads | psql -d mydb -h pgm-xxxxxxx.pg.rds.aliyuncs.com -U my_name -p 3433
    ```

    **Note:** If you want to import more than one vector data file at a time by using a script, run the `export PGPASSWORD=my_pass` command to set the PGPASSWORD environment variable to my\_pass.


## Load vector data by using ogr2ogr

The ogr2ogr command line tool is provided by GDAL/OGR to convert data. It supports common vector data types, such as Esri ShapFile, MapInfo, and FileGDB. For more information about ogr2ogr, visit [ogr2ogr](https://postgis.net/docs/using_postgis_dbmanagement.html#idm2274). For more information anout vector data types, visit [Vector drivers](https://gdal.org/drivers/vector/index.html).

**Note:** Before you load vector data by using the ogr2ogr command line tool, make sure that GDAL supports the PostGIS vector driver.

-   Syntax

    ```
    ogr2ogr -nln <table_name> -f PostgreSQL PG:"dbname='<dbname>' host='<host>' port='<port>' user='<user_name>' password='<password>'" -lco SPATIAL_INDEX=GIST -lco FID=<FID_NAME> -overwrite "<PATH_TO_FILE>" -nlt GEOMETRY
    ```

-   Parameters

    |Parameter|Description|Example|
    |---------|-----------|-------|
    |-nln <table\_name\>|The name of the vector data file.|roads|
    |-f PostgreSQL PG:"dbname='<dbname\>' host='<host\>' port='<port\>' user='<user\_name\>' password='<password\>'"|The information that is used to connect to the AnalyticDB for PostgreSQL instance.|dbname='mydb' host='pgm-xxxxxxx.pg.rds.aliyuncs.com' port='3433' user='my\_name' password='my\_pass'|
    |-lco SPATIAL\_INDEX=GIST|The GiST spatial index that you want to create.|None|
    |-lco FID=<FID\_NAME\>|The ID of the specified feature.|fid|
    |-overwrite|The rewrite mode that you want to use.You can also select the -append mode.

|None|
    |<PATH\_TO\_FILE\>|The storage path of the vector data file. Make sure that the storage path is accessible to the ogr2ogr command line tool.|/home/roads.shp /home/land.tab |
    |-nlt GEOMETRY|Specifies the geometry data type. If the vector data file contains polygon data, we recommend that you specify this parameter. This allows you to avoid errors that occur if the vector data file contains not only data of the MultiPolygon type but also data of other types.|None|

-   Example

    ```
    ogr2ogr -nln roads -f PostgreSQL PG:"dbname='mydb' host='pgm-xxxxxxx.pg.rds.aliyuncs.com' port='3433' user='my_name' password='my_pass'" -lco SPATIAL_INDEX=GIST -lco FID=fid -overwrite "/home/roads.shp" -nlt GEOMETRY
    ```


## Load vector data by using QGIS

QGIS was known as Quantum GIS. It is a user-friendly, open-source desktop tool that supports various vector and raster formats. It allows you to use databases as data sources. In addition, it allows you to visualize, manage, and analyze data and to prepare maps. For more information, see [QGIS](https://www.qgis.org/en/site/).

1.  Establish a connection to Ganos.

    1.  In the right-side **Browser** pane, create a PostGIS connection.

    2.  Configure the required parameters.

        ![Load vector data into Ganos - QGIS - Connect to Ganos](https://static-aliyun-doc.oss-accelerate.aliyuncs.com/assets/img/en-US/5125911061/p148423.png)

        The following table describes the parameters.

        |Parameter|Description|
        |---------|-----------|
        |**Name**|The name of the connection.|
        |**Host**|The IP address of your ECS instance or the public endpoint of your ApsaraDB RDS instance or PolarDB cluster. You can obtain this information from the Alibaba Cloud Management Console.|
        |**Port**|The port number of your ECS instance, ApsaraDB RDS instance, or PolarDB cluster. You can obtain this information from the Alibaba Cloud Management Console.|
        |**Database**|The name of the database.|
        |**SSL Mode**|Specifies whether to establish an encrypted connection based on SSL.|

    3.  Click **Test Connection**. Then, wait until the connection is established.

2.  Load the vector data file.

    1.  In the menu bar, choose **Database** \> **DB Manager**.

    2.  In the dialog box that appears, select the data source from which you want to import the vector data file, and click **Import Layer/File...**.

    3.  In the dialog box that appears, click the vector data file, configure the required parameters, and then click **OK**.


