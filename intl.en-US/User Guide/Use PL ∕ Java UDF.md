# Use PL ∕ Java UDF {#concept_rpl_x3z_52b .concept}

AnalyticDB for PostgreSQL supports compiling and uploading JAR software packages written in PL/Java languages, and using these JAR packages to create user-defined functions \(UDF\). The PL/Java language supported in this feature is Community Edition PL/Java 1.5.0 and the JVM version is 1.8.

This document describes how to create a PL/Java UDF. For more PL/Java examples, see [PL/Java Code](https://github.com/tada/pljava/tree/master/pljava-examples/src/main/java/org/postgresql/pljava/example). You can also view [How to Compile](https://tada.github.io/pljava/build/build.html).

## Procedure { .section}

1.  In AnalyticDB for PostgreSQL, run the following command to create a PL/Java plug-in. The command only needs to be ran once for the database.

    ```
    create extension pljava;
    ```

2.  Compile the UDF based on your business requirements. For example, you can use the following code to compile the `Test.java` file:

    ```
    public class Test
    {
     public static String substring(String text, int beginIndex,
               int endIndex)
             {
                     return text.substring(beginIndex, endIndex);
             }
    }
    ```

3.  Compile the `manifest.txt` file.

    ```
    Manifest-Version: 1.0
    Main-Class: Test
    Specification-Title: "Test"
    Specification-Version: "1.0"
    Created-By: 1.7.0_99
    Build-Date: 01/20/2016 21:00 AM
    ```

4.  Run the following command to compile and package the program.

    ```
    javac Test.java
    jar cfm analytics.jar manifest.txt Test.class
    ```

5.  Upload the `analytics.jar` file generated in Step 4 to the OSS by using the following OSS console command.

    ```
    osscmd put analytics.jar oss://zzz
    ```

6.  In AnalyticDB for PostgreSQL, run the “Create Library” command to import the file to AnalyticDB for PostgreSQL.

    **Note:** The `Create Library` command only supports filepath and you can import one file a time. In addition, the “Create Library” command also supports byte streams to directly import files without using the OSS. For more information, see [Use the Create Library Command](intl.en-US/User Guide/Use the create Library command.md#).

    ```
    create library example language java from 'oss://oss-cn-hangzhou.aliyuncs.com filepath=analytics.jar id=xxx key=yyy bucket=zzz';
    ```

7.  In AnalyticDB for PostgreSQL, run the following command to create and use the UDF.

    ```
    create table temp (a varchar) distributed randomly;
    insert into temp values ('my string');
    create or replace function java_substring(varchar, int, int) returns varchar as 'Test.substring' language java;
    select java_substring(a, 1, 5) from temp;
    ```


