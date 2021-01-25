# Use the CREATE LIBRARY statement

AnalyticDB for PostgreSQL introduces the CREATE LIBRARY and DROP LIBRARY statements to allow you to import custom software packages. For more information, see [Use PL/Java UDF](/intl.en-US/Advanced Developer Guide/Use PL/Java/Use PL/Java UDF.md).

This topic describes how to use the CREATE LIBRARY and DROP LIBRARY statements.

## Syntax

```
CREATE LIBRARY library_name LANGUAGE [JAVA] FROM oss_location OWNER ownername
CREATE LIBRARY library_name LANGUAGE [JAVA] VALUES file_content_hex OWNER ownername
DROP LIBRARY library_name
```

**Parameter description**:

-   `library_name`: the name of the library to be installed. If the library to be installed has the same name as an existing library, you must delete the existing library before you install the new library.
-   `LANGUAGE [JAVA]`: the programming language to be used. Only PL/Java is supported.
-   `oss_location`: the location of the package. You can specify an Object Storage Service \(OSS\) bucket and an object name. Only one object can be specified and the specified object cannot be a compressed file. Sample format:

    ```
    oss://oss_endpoint filepath=[folder/[folder/]...]/file_name id=userossid key=userosskey bucket=ossbucket
    ```

    You can also use a temporary Security Token Service \(STS\) credential to access an OSS bucket. For more information, see [Access OSS with a temporary access credential provided by STS](/intl.en-US/Developer Guide/Data security/Access and control/Access OSS with a temporary access credential provided by STS.md). Sample format:

    ```
    oss://oss_endpoint filepath=[folder/[folder/]...]/file_name id=userossid key=userosskey token=usersecuritytoken bucket=ossbucket
    ```

-   `file_content_hex`: the content of the file. The byte stream is in hexadecimal notation. For example, `73656c6563742031` indicates the hexadecimal byte stream of "select 1". You can use this syntax to import packages without the need to use OSS.
-   `ownername`: specifies the name of the user.
-   `DROP LIBRARY`: deletes a library.

## Examples

-   Example 1: Install a JAR package that is named `analytics.jar`.

    ```
    create library example language java from 'oss://oss-cn-hangzhou.aliyuncs.com filepath=analytics.jar id=xxx key=yyy bucket=zzz';
    ```

-   Example 2: Use a temporary STS credential to install a JAR package that is named `analytics.jar`.

    ```
    create library example language java from 'oss://oss-cn-hangzhou.aliyuncs.com filepath=analytics.jar id=xxx key=yyy token=ttt bucket=zzz';
    ```

-   Example 3: Import file content of which the byte stream is in hexadecimal notation.

    ```
    create library  pglib LANGUAGE java VALUES '73656c6563742031' OWNER "myuser";
    ```

-   Example 4: Delete a library.

    ```
    drop library example;
    ```

-   Example 5: View installed libraries.

    ```
    select name, lanname from pg_library;
    ```


