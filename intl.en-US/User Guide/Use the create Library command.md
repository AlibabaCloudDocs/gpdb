# Use the create Library command {#concept_prd_qhz_52b .concept}

AnalyticDB for PostgreSQL introduces the “Create Library/Drop Library” command to allow you to import custom software packages. For PL/Java UDF examples created by using this command, see [PL/Java UDF Usage](intl.en-US/User Guide/Use PL ∕ Java UDF.md#).

This document describes the usage of the `Create/Drop Library` command.

## Syntax { .section}

```
CREATE LIBRARY library_name LANGUAGE [JAVA] FROM oss_location OWNER ownername
CREATE LIBRARY library_name LANGUAGE [JAVA] VALUES file_content_hex OWNER ownername
DROP LIBRARY library_name
```

**Parameter description**:

-   `library_name`: name of the library to be installed. If the name of the library to be installed conflicts with an existing library’s name, the existing library must be deleted first to install the new one.
-   `LANGUAGE [JAVA]`: the language to use. Currently only PL/Java is supported.
-   `oss_location`: location of the package file. You can specify the OSS bucket and object name. Only one object can be specified and the specified object must not be a compressed file. The format is:

    ```
    oss://oss_endpoint filepath=[folder/[folder/]...]/file_name id=userossid key=userosskey bucket=ossbucket
    ```

-   `file_content_hex`: file content. The byte stream is in hexadecimal notation. For example, `73656c6563742031` indicates the hexadecimal byte stream of “select 1”. With this syntax, you can directly import package files without using the OSS.
-   `ownername`: specify the user.
-   `DROP LIBRARY`: delete a library.

## Examples { .section}

-   Install a JAR package named `analytics.jar`.

    ```
    create library example language java from 'oss://oss-cn-hangzhou.aliyuncs.com filepath=analytics.jar id=xxx key=yyy bucket=zzz';
    ```

-   Import the file content directly and the byte stream is in hexadecimal notation.

    ```
    create library  pglib LANGUAGE java VALUES '73656c6563742031' OWNER "myuser";
    ```

-   Delete a library.

    ```
    drop library example;
    ```

-   View installed libraries.

    ```
    select name, lanname from pg_library;
    ```


