# 使用 Create Library 命令 {#concept_prd_qhz_52b .concept}

为支持用户导入自定义软件包，HybridDB for PostgreSQL 引入了 Create Library/Drop Library 命令。使用此命令创建 PL/Java 的 UDF 的示例，请参见 [PL/Java UDF的使用](intl.zh-CN/用户指南/使用 PL∕Java UDF.md#)。

本文介绍了 Create/Drop Library 命令的使用方法。

## 语法 {#section_xs4_5hz_52b .section}

```
CREATE LIBRARY library_name LANGUAGE [JAVA] FROM oss_location OWNER ownername
CREATE LIBRARY library_name LANGUAGE [JAVA] VALUES file_content_hex OWNER ownername
DROP LIBRARY library_name
```

**参数说明**：

-   `library_name`：要安装的库的名称。若已安装的库与要安装的库的名称相同，必须先删除现有的库，然后再安装新库。
-   `LANGUAGE [JAVA]`：要使用的语言。目前仅支持 PL/Java。
-   `oss_location`：包文件的位置。您可以指定 OSS 存储桶和对象名称，仅可以指定一个文件，且不能为压缩文件。其格式为：

    ```
    oss://oss_endpoint filepath=[folder/[folder/]...]/file_name id=userossid key=userosskey bucket=ossbucket
    ```

-   `file_content_hex`：文件内容，字节流为 16 进制。例如，`73656c6563742031`表示”select 1”的 16 进制字节流。借助这个语法，可以直接导入包文件，不必通过 OSS。
-   `ownername`：指定用户。
-   `DROP LIBRARY`：删除一个库。

## 示例 {#section_z4v_zhz_52b .section}

-   示例 1：安装名为 `analytics.jar` 的 jar 包。

    ```
    create library example language java from 'oss://oss-cn-hangzhou.aliyuncs.com filepath=analytics.jar id=xxx key=yyy bucket=zzz';
    ```

-   示例 2：直接导入文件内容，字节流为 16 进制。

    ```
    create library  pglib LANGUAGE java VALUES '73656c6563742031' OWNER "myuser";
    ```

-   示例 3：删除一个库。

    ```
    drop library example;
    ```

-   示例 4：查看已经安装的库。

    ```
    select name, lanname from pg_library;
    ```


