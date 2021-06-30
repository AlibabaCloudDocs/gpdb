# Data types

This topic provides an overview of the data types supported by AnalyticDB for PostgreSQL. You can also create a data type by executing a CREATE TYPE statement.

## Data types supported

The following table lists the data types supported by AnalyticDB for PostgreSQL.

|Data type|Alias|Length|Range|Description|
|:--------|:----|:-----|:----|:----------|
|bigint|int8|8 bytes|-922337203​6854775808 to 922337203​6854775807|An integer within a large range.|
|bigserial|serial8|8 bytes|1 to 922337203​6854775807|A large auto-increment integer.|
|bit \[ \(n\) \]| |n bits|Bit string constant|A bit string with a fixed length.|
|bit varying \[ \(n\) \]|varbit|Variable|Bit string constant|A bit string with a variable length.|
|boolean|bool|1 byte|true/false, t/f, yes/no, y/n, 1/0|A boolean value \(true or false\).|
|box| |32 bytes|\(\(x1,y1\),\(x2,y2\)\)|A rectangular box on a plane, not allowed in a column that is used as the distribution key.|
|bytea| |1 byte + binary string|Sequence of octets|A binary string with a variable length.|
|character \[ \(n\) \]|char \[ \(n\) \]|1 byte + n|String up to n characters in length|A blank-padded string with a fixed length.|
|character varying \[ \(n\) \]|varchar \[ \(n\) \]|1 byte + string size|String up to n characters in length|A string with a limited variable length.|
|cidr| |12 or 24 bytes| |IPv4 and IPv6 networks.|
|circle| |24 bytes|<\(x,y\),r\> \(center and radius\)|A circle on a plane, not allowed in a column that is used as the distribution key.|
|date| |4 bytes|4713 BC to 294,277 AD|A calendar date \(year, month, day\).|
|decimal \[ \(p, s\) \]|numeric \[ \(p, s\) \]|Variable|Unlimited|User-specified precision, which is accurate.|
|double precision|float8|8 bytes|15 digits|Variable precision, which is inaccurate.|
|float|
|inet| |12 or 24 bytes| |IPv4 and IPv6 hosts and networks.|
|Integer|int or int4|4 bytes|-2.1E+09 to +2147483647|An integer in typical cases.|
|interval \[ \(p\) \]| |12 bytes|-178000000 years to 178000000 years|A time range.|
|json| |1 byte + JSON size|JSON string|A string with an unlimited variable length.|
|lseg| |32 bytes|\(\(x1,y1\),\(x2,y2\)\)|A line segment on a plane, not allowed in a column that is used as the distribution key.|
|macaddr| |6 bytes| |A MAC address.|
|money| |8 bytes|-92233720368547758.08 to +92233720368547758.07|An amount of money.|
|path| |16+16n bytes|\[\(x1,y1\),...\]|A geometric path on a plane, not allowed in a column that is used as the distribution key.|
|point| |16 bytes|\(x,y\)|A geometric point on a plane, not allowed in a column that is used as the distribution key.|
|polygon| |40+16n bytes|\(\(x1,y1\),...\)|A closed geometric path on a plane, not allowed in a column that is used as the distribution key.|
|real|float4|4 bytes|6 digits|Variable precision, which is inaccurate.|
|serial|serial4|4 bytes|1 to 2147483647|An auto-increment integer.|
|smallint|int2|2 bytes|-32768 to +32767|An integer within a small range.|
|text| |1 byte + string size|Unlimited|A string with an unlimited variable length.|
|time \[ \(p\) \] \[ without time zone \]| |8 bytes|00:00:00\[.000000\] to 24:00:00\[.000000\]|The time of a day without the time zone.|
|time \[ \(p\) \] with time zone|timetz|12 bytes|00:00:00+1359 to 24:00:00-1359|The time of a day with the time zone.|
|timestamp \[ \(p\) \] \[ without time zone \]| |8 bytes|4713 BC to 294,277 AD|The date and time without the time zone.|
|timestamp \[ \(p\) \] with time zone|timestamptz|8 bytes|4713 BC to 294,277 AD|The date and time with the time zone.|
|xml| |1 byte + XML size|Unlimited|A string with an unlimited variable length.|
|uuid| |32 bytes| |The uuid data type is provided with AnalyticDB for PostgreSQL V6.0. In AnalyticDB for PostgreSQL V4.3, however, you must install the uuid-ossp extension before you can use the uuid data type. For more information, see [Use the uuid-ossp extension](/intl.en-US/Advanced Developer Guide/Use advanced extensions/Use the uuid-ossp extension.md).|

## References

For more information, visit [Pivotal Greenplum documentation](https://gpdb.docs.pivotal.io/6-1/ref_guide/data_types.html).

