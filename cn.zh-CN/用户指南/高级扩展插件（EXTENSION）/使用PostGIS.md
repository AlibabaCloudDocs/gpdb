# 使用PostGIS {#concept_1425913 .concept}

PostGIS是数据库PostgreSQL的一个扩展，PostGIS提供如下空间信息服务功能：空间对象、空间索引、空间操作函数和空间操作符。

**说明：** 

-   PostGIS遵循OpenGIS的规范。
-   AnalyticDB for PostgreSQL支持空间数据库PostGIS（目前版本2.0.3）。

## 安装PostGIS插件 {#section_t18_s07_m5q .section}

使用如下代码安装PostGIS插件：

``` {#codeblock_l7w_msq_oqp}
CREATE EXTENSION PostGIS;
```

## 查看版本 {#section_lb7_o72_ard .section}

使用如下代码查看PostGIS插件版本：

``` {#codeblock_ovr_bg8_s00}
SELECT PostGIS_full_version();
```

## 示例 {#section_pun_wq9_zqk .section}

**说明：** 以下经纬度数据均来自[高德地图开放平台](https://lbs.amap.com/console/show/picker)。

-   **创建表并导入数据** 
    1.  创建表`hz_metro`表示杭州地铁，并导入数据。

        **说明：** 示例仅采集地铁1号线和2号线数据。

        ``` {#codeblock_dds_d49_26w}
        DROP TABLE IF EXISTS hz_metro;
        
        CREATE TABLE hz_metro(
            lineno integer,             -- 地铁线
            stat_id integer,            -- 地铁站编号
            stat_name varchar(30),      -- 地铁站名
            longitude float8,           -- 经度
            latitude float8             -- 维度
        ) DISTRIBUTED BY (stat_name);
        
        COPY hz_metro FROM STDIN;
        1 1 湘湖  120.234391  30.167585
        1 2 滨康路 120.231003  30.183864
        1 3 西兴  120.220429  30.187295
        1 4 滨和路 120.217552  30.19955
        1 5 江陵路 120.216602  30.208994
        1 6 近江  120.197851  30.230791
        1 7 婺江路 120.191008  30.236914
        1 8 城站  120.181104  30.244457
        1 9 定安路 120.167751  30.245954
        1 10  龙翔桥 120.164052  30.254642
        1 11  凤起路 120.162887  30.263779
        1 12  武林广场  120.164324  30.272368
        1 13  西湖文化广场  120.165639  30.279585
        1 14  打铁关 120.176592  30.285421
        1 15  闸弄口 120.192496  30.284507
        1 16  火车东站  120.212892  30.291124
        1 17  彭埠  120.223661  30.294065
        1 18  七堡  120.241065  30.300002
        1 19  九和路 120.252784  30.305799
        1 20  九堡  120.266622  30.307910
        1 21  客运中心  120.278700  30.311183
        1 22  下沙西 120.312534  30.309918
        1 23  金沙湖 120.325720  30.309246
        1 24  高沙路 120.335408  30.30942
        1 25  文泽路 120.348439  30.309996
        1 26  文海南路  120.376146  30.311208
        1 27  云水  120.381787  30.302893
        1 28  下沙江滨  120.381720  30.287736
        2 1 良渚  120.041654  30.358992
        2 2 杜甫村 120.056549  30.359033
        2 3 白洋  120.072905  30.351155
        2 4 金家渡 120.083912  30.339138
        2 5 墩祥街 120.087615  30.331667
        2 6 三墩  120.093014  30.320033
        2 7 虾龙圩 120.096383  30.309587
        2 8 三坝  120.097900  30.300020
        2 9 文新  120.098193  30.289251
        2 10  丰潭路 120.109629  30.281911
        2 11  古翠路 120.117918  30.282192
        2 12  学院路 120.129776  30.282757
        2 13  下宁桥 120.140110  30.283321
        2 14  沈塘桥 120.150223  30.280146
        2 15  武林门 120.155334  30.269792
        2 16  凤起路 120.162887  30.263779
        2 17  中河北路  120.172436  30.264596
        2 18  建国北路  120.181413  30.264496
        2 19  庆菱路 120.193077  30.257613
        2 20  庆春广场  120.204748  30.257602
        2 21  钱江路 120.213387  30.255978
        2 22  钱江世纪城 120.244139  30.240555
        2 23  盈丰路 120.252501  30.236997
        2 24  飞虹路 120.263328  30.230955
        2 25  振宁路 120.269215  30.217695
        2 26  建设三路  120.268252  30.204512
        2 27  建设一路  120.266538  30.194046
        2 28  人民广场  120.266831  30.179593
        2 29  杭发厂 120.267205  30.170619
        2 30  人民路 120.267502  30.159176
        2 31  潘水  120.267644  30.148186
        2 32  曹家桥 120.267867  30.134524
        2 33  朝阳  120.268228  30.122219
        \.
        ```

    2.  创建表`hz_traffic`表示杭州交通要点，并导入数据。

        ``` {#codeblock_u2t_3k5_g38}
        DROP TABLE IF EXISTS hz_traffic;
        
        CREATE TABLE hz_traffic(
            traf_id integer,             -- 交通站点编号
            traf_name varchar(30),       -- 交通站点名称
            longitude float8,            -- 经度
            latitude float8              -- 维度
        ) DISTRIBUTED BY(traf_name);
        
        COPY hz_traffic FROM STDIN;
        1 杭州站 120.182802  30.243319
        2 杭州东站  120.213116  30.290998
        3 杭州汽车客运中心站 120.279000  30.313103
        4 杭州萧山国际机场  120.432414  30.234708
        \.
        ```

    3.  利用经纬度分别为表`hz_metro`和表`hz_traffic`添加`Geometry`字段。

        ``` {#codeblock_hg5_9xr_sxp}
        -- 表hz_metro添加stat_geom POINT类型字段，维度为2，SRID 4326(WGS-84 空间坐标系统)
        SELECT AddGeometryColumn ('public','hz_metro','stat_geom',4326,'POINT',2);
        -- 通过各地铁站经纬度生成相应空间字段数据
        UPDATE hz_metro SET stat_geom = 
            ST_GeomFromEWKT('SRID=4326;POINT(' || longitude || ' ' || latitude || ')');
        -- 创建GIST索引
        CREATE INDEX idx_hz_metro_stat_geom ON hz_metro USING gist(stat_geom);
        
        -- 表hz_traffic添加traf_geom POINT类型字段，维度为2，SRID 4326(WGS-84 空间坐标系统)
        SELECT AddGeometryColumn ('public','hz_traffic','traf_geom',4326,'POINT',2);
        -- 通过各交通站点经纬度生成相应空间字段数据
        UPDATE hz_traffic SET traf_geom =
            ST_GeomFromEWKT('SRID=4326;POINT(' || longitude || ' ' || latitude || ')');
        -- 创建GIST索引
        CREATE INDEX idx_hz_traffic_traf_geom ON hz_traffic USING gist(traf_geom);
        ```

-   **查询示例** 
    -   打铁关地铁站到杭州东站的地理距离：

        ``` {#codeblock_ylb_15k_ojw}
        select ST_Distance(a.stat_geom::geography, b.traf_geom)
          from hz_metro a, hz_traffic b
         where a.stat_name = '打铁关'
           and b.traf_name = '杭州东站';
        ```

        SQL结果如下：

        ``` {#codeblock_1kh_1gb_loe}
        mydb=# select ST_Distance(a.stat_geom::geography, b.traf_geom)
        mydb-#   from hz_metro a, hz_traffic b
        mydb-#  where a.stat_name = '打铁关'
        mydb-#    and b.traf_name = '杭州东站';
           st_distance
        ------------------
         3567.81261626845
        (1 row)
        ```

        对比高德地图测距结果。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1135059/156436614353658_zh-CN.png)

    -   杭州市儿童医院附近1KM内的地铁站：

        **说明：** 杭州市儿童医院经纬度：\(120.170851,30.280955\)。

        ``` {#codeblock_7s2_1xe_biy}
        -- 利用ST_Distance函数计算地理距离
        select lineno, stat_id, stat_name
          from hz_metro
         where ST_Distance(ST_GeomFromEWKT('SRID=4326;POINT(120.170851 30.280955)')::geography,
                           stat_geom::geography) <= 1000
         order by 1, 2;
        
        -- 利用ST_DWithin函数
        select lineno, stat_id, stat_name
          from hz_metro
         where ST_DWithin(ST_GeomFromEWKT('SRID=4326;POINT(120.170851 30.280955)')::geography,
                          stat_geom::geography, 1000)
         order by 1, 2;
        ```

        SQL结果如下：

        ``` {#codeblock_zse_0a3_vb1}
        mydb=# select lineno, stat_id, stat_name
        mydb-#   from hz_metro
        mydb-#  where ST_Distance(ST_GeomFromEWKT('SRID=4326;POINT(120.170851 30.280955)')::geography,
        mydb(#                    stat_geom::geography) <= 1000
        mydb-#  order by 1, 2;
         lineno | stat_id |  stat_name
        --------+---------+--------------
              1 |      13 | 西湖文化广场
              1 |      14 | 打铁关
        (2 rows)
        
        mydb=# select lineno, stat_id, stat_name
        mydb-#   from hz_metro
        mydb-#  where ST_DWithin(ST_GeomFromEWKT('SRID=4326;POINT(120.170851 30.280955)')::geography,
        mydb(#                   stat_geom::geography, 1000)
        mydb-#  order by 1, 2;
         lineno | stat_id |  stat_name
        --------+---------+--------------
              1 |      13 | 西湖文化广场
              1 |      14 | 打铁关
        (2 rows)
        ```

        对比高德地图搜索结果。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/1135059/156436614453662_zh-CN.png)


## 参考资料 {#section_z5v_113_ju2 .section}

-   SRID \(空间引用识别号, 坐标系\)：

    [https://yq.aliyun.com/articles/137044](https://yq.aliyun.com/articles/137044)

    **说明：** `spatial_ref_sys`坐标系导入需联系阿里工程师。

-   PostGIS官方介绍文档：

    [https://postgis.net/workshops/postgis-intro/index.html](https://postgis.net/workshops/postgis-intro/index.html)


