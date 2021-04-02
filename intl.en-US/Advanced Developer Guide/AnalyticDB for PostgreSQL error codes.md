AnalyticDB for PostgreSQL error codes 
==========================================================

Each message that is sent by the AnalyticDB for PostgreSQL server is assigned a five-character error code that follows the conventions of the SQL standard for SQLSTATE codes. This topic describes all the error codes that are defined in PostgreSQL 8.1. 


| Error code |                                                    Description                                                    |
|------------|-------------------------------------------------------------------------------------------------------------------|
| Class 00: successful operation.                                                                                               ||
| 00000      | The error code returned because the operation is successful.                                                      |
| Class 01: warning.                                                                                                            ||
| 01000      | The error code returned to indicate a warning.                                                                    |
| 0100C      | The error code returned because dynamic result sets are returned.                                                 |
| 01008      | The error code returned because the value is padded with 0-bits.                                                  |
| 01003      | The error code returned because the NULL value is eliminated in the set function.                                 |
| 01007      | The error code returned because the required permissions are not granted.                                         |
| 01006      | The error code returned because the specified permissions are not revoked.                                        |
| 01004      | The error code returned because the string is truncated on the right.                                             |
| 01P01      | The error code returned because the feature is deprecated.                                                        |
| Class 02: no data. This is also a warning class as per the SQL standard.                                                      ||
| 02000      | The error code returned because no data is returned.                                                              |
| 02001      | The error code returned because no additional dynamic result sets are returned.                                   |
| Class 03: The execution of the SQL statement is not complete.                                                                 ||
| 03000      | The error code returned because the execution of the SQL statement is not complete.                               |
| Class 08: connection exception.                                                                                               ||
| 08000      | The error code returned because a connection exception occurs.                                                    |
| 08003      | The error code returned because a connection does not exist.                                                      |
| 08006      | The error code returned because a connection failure occurs.                                                      |
| 08001      | The error code returned because the SQL client fails to establish a connection.                                   |
| 08004      | The error code returned because the SQL server rejects the request from the SQL client to establish a connection. |
| 08007      | The error code returned because the resolution for the transaction is unknown.                                    |
| 08P01      | The error code returned because the protocol is violated.                                                         |
| Class 09: trigger action exception.                                                                                           ||
| 09000      | The error code returned because an exception is thrown when an action is triggered.                               |
| Class 0A: feature not supported.                                                                                              ||
| 0A000      | The error code returned because the feature is not supported.                                                     |
| Class 0B: invalid initialization of transactions.                                                                             ||
| 0B000      | The error code returned because the transaction initialization is invalid.                                        |
| Class 0F: locator exception.                                                                                                  ||
| 0F000      | The error code returned because a locator exception occurs.                                                       |
| 0F001      | The error code returned because the locator specification is invalid.                                             |
| Class 0L: invalid grantor.                                                                                                    ||
| 0L000      | The error code returned because the grantor is invalid.                                                           |
| 0LP01      | The error code returned because the grant operation is invalid.                                                   |
| Class 0P: invalid role specification.                                                                                         ||
| 0P000      | The error code returned because the role specification is invalid.                                                |
| Class 21: cardinality violation.                                                                                              ||
| 21000      | The error code returned because cardinality violations occur.                                                     |
| Class 22: data exception.                                                                                                     ||
| 22000      | The error code returned because a data exception occurs.                                                          |
| 2202E      | The error code returned because the array subscript is invalid.                                                   |
| 22021      | The error code returned because characters are not in repertoire.                                                 |
| 22008      | The error code returned because the datetime field overflows.                                                     |
| 22012      | The error code returned because a number is divided by zero.                                                      |
| 22005      | The error code returned because an error occurs in assignment.                                                    |
| 2200B      | The error code returned because an escape character conflict occurs.                                              |
| 22022      | The error code returned because the indicator overflows.                                                          |
| 22015      | The error code returned because an internal field overflows.                                                      |
| 2201E      | The error code returned because the logarithm operation has invalid arguments.                                    |
| 2201F      | The error code returned because the power function has invalid arguments.                                         |
| 2201G      | The error code returned because the WIDTH_BUCKET function has invalid arguments.                                  |
| 22018      | The error code returned because the cast specification has invalid character values.                              |
| 22007      | The error code returned because the datetime format is invalid.                                                   |
| 22019      | The error code returned because the escape character is invalid.                                                  |
| 2200D      | The error code returned because the escape octet is invalid.                                                      |
| 22025      | The error code returned because the escape sequence is invalid.                                                   |
| 22P06      | The error code returned because a non-standard escape character is used.                                          |
| 22010      | The error code returned because the indicator parameter value is invalid.                                         |
| 22020      | The error code returned because the limit value is invalid.                                                       |
| 22023      | The error code returned because the parameter value is invalid.                                                   |
| 2201B      | The error code returned because the regular expression is invalid.                                                |
| 22009      | The error code returned because the time zone displacement value is invalid.                                      |
| 2200C      | The error code returned because an invalid escape character is used.                                              |
| 2200G      | The error code returned because the most relevant types do not match.                                             |
| 22004      | The error code returned because NULL values are not allowed.                                                      |
| 22002      | The error code returned because NULL values cannot be used for indicator parameters.                              |
| 22003      | The error code returned because the numeric value is out of range.                                                |
| 22026      | The error code returned because lengths of strings do not match.                                                  |
| 22001      | The error code returned because the string is truncated on the right.                                             |
| 22011      | The error code returned because a substring error occurs.                                                         |
| 22027      | The error code returned because a truncation error occurs.                                                        |
| 22024      | The error code returned because the C string is not terminated.                                                   |
| 2200F      | The error code returned because a zero-length character string is returned.                                       |
| 22P01      | The error code returned because a floating point exception occurs.                                                |
| 22P02      | The error code returned because the text representation is invalid.                                               |
| 22P03      | The error code returned because the binary representation is invalid.                                             |
| 22P04      | The error code returned because the copy file format is invalid.                                                  |
| 22P05      | The error code returned because untranslatable characters are found.                                              |
| Class 23: integrity constraint violation.                                                                                     ||
| 23000      | The error code returned because the integrity constraint is violated.                                             |
| 23001      | The error code returned because the limit is violated.                                                            |
| 23502      | The error code returned because the NOT NULL rule is violated.                                                    |
| 23503      | The error code returned because the foreign key constraint is violated.                                           |
| 23505      | The error code returned because the constraint on uniqueness is violated.                                         |
| 23514      | The error code returned because the check constraint is violated.                                                 |
| Class 24: invalid cursor state.                                                                                               ||
| 24000      | The error code returned because the cursor state is invalid.                                                      |
| Class 25: invalid transaction state.                                                                                          ||
| 25000      | The error code returned because the transaction state is invalid.                                                 |
| 25001      | The error code returned because the SQL transaction is active.                                                    |
| 25002      | The error code returned because the branch transaction is active.                                                 |
| 25008      | The error code returned because the held cursors require the same isolation level.                                |
| 25003      | The error code returned because the access mode for the branch transaction is inappropriate.                      |
| 25004      | The error code returned because the isolation level for the branch transaction is inappropriate.                  |
| 25005      | The error code returned because the branch transaction does not have an active SQL transaction.                   |
| 25006      | The error code returned because the SQL transaction is read-only.                                                 |
| 25007      | The error code returned because the schema and data statement cannot be mixed.                                    |
| 25P01      | The error code returned because no active SQL transactions exist.                                                 |
| 25P02      | The error code returned because the SQL transaction is in the failed state.                                       |
| Class 26: invalid SQL statement name.                                                                                         ||
| 26000      | The error code returned because the SQL statement name is invalid.                                                |
| Class 27: triggered data change violation.                                                                                    ||
| 27000      | The error code returned because the triggered data change violates constraints.                                   |
| Class 28: invalid authorization specification.                                                                                ||
| 28000      | The error code returned because the authorization specification is invalid.                                       |
| Class 2B: Dependent privilege descriptors still exist.                                                                        ||
| 2B000      | The error code returned because dependent privilege descriptors still exist.                                      |
| 2BP01      | The error code returned because dependent objects still exist.                                                    |
| Class 2D: invalid transaction termination.                                                                                    ||
| 2D000      | The error code returned because the transaction termination is invalid.                                           |
| Class 2F: SQL routine exception.                                                                                              ||
| 2F000      | The error code returned because an SQL routine exception occurs.                                                  |
| 2F005      | The error code returned because the executed function does not return statements.                                 |
| 2F002      | The error code returned because the SQL data cannot be modified.                                                  |
| 2F003      | The error code returned because the SQL statements to be used are prohibited.                                     |
| 2F004      | The error code returned because the SQL data cannot be read.                                                      |
| Class 34: invalid cursor name.                                                                                                ||
| 34000      | The error code returned because the cursor name is invalid.                                                       |
| Class 38: external routine exception.                                                                                         ||
| 38000      | The error code returned because an external routine exception occurs.                                             |
| 38001      | The error code returned because prohibited SQL statements are contained.                                          |
| 38002      | The error code returned because the SQL data cannot be modified.                                                  |
| 38003      | The error code returned because the SQL statements to be used are prohibited.                                     |
| 38004      | The error code returned because the SQL data cannot be read.                                                      |
| Class 39: exception in external routine invocation.                                                                           ||
| 39000      | The error code returned because an exception occurs during external routine invocation.                           |
| 39001      | The error code returned because the returned SQL statement is invalid.                                            |
| 39004      | The error code returned because NULL values are not allowed.                                                      |
| 39P01      | The error code returned because the trigger protocol is violated.                                                 |
| 39P02      | The error code returned because the SRF protocol is violated.                                                     |
| Class 3B: savepoint exception.                                                                                                ||
| 3B000      | The error code returned because a savepoint exception occurs.                                                     |
| 3B001      | The error code returned because the savepoint specification is invalid.                                           |
| Class 3D: invalid database name.                                                                                              ||
| 3D000      | The error code returned because the database name is invalid.                                                     |
| Class 3F: invalid schema name.                                                                                                ||
| 3F000      | The error code returned because the schema name is invalid.                                                       |
| Class 40: transaction rollback.                                                                                               ||
| 40000      | The error code returned because the transaction is rolled back.                                                   |
| 40002      | The error code returned because the constraint on transaction integrity is violated.                              |
| 40001      | The error code returned because the serialization fails.                                                          |
| 40003      | The error code returned because whether the statement execution is complete is unknown.                           |
| 40P01      | The error code returned because a deadlock is detected.                                                           |
| Class 42: syntax error or access rule violation                                                                               ||
| 42000      | The error code returned because a syntax error occurs or the access rule is violated.                             |
| 42601      | The error code returned because a syntax error occurs.                                                            |
| 42501      | The error code returned because the required permissions are not granted.                                         |
| 42846      | The error code returned because the data type cannot be converted.                                                |
| 42803      | The error code returned because a grouping error occurs.                                                          |
| 42830      | The error code returned because the foreign key is invalid.                                                       |
| 42602      | The error code returned because the name is invalid.                                                              |
| 42622      | The error code returned because the length of the name exceeds the limit.                                         |
| 42939      | The error code returned because the name is reserved.                                                             |
| 42804      | The error code returned because data types do not match.                                                          |
| 42P18      | The error code returned because the data type is indeterminate.                                                   |
| 42809      | The error code returned because the object type is invalid.                                                       |
| 42703      | The error code returned because the column is undefined.                                                          |
| 42883      | The error code returned because the function is undefined.                                                        |
| 42P01      | The error code returned because the table is undefined.                                                           |
| 42P02      | The error code returned because the parameter is undefined.                                                       |
| 42704      | The error code returned because the object is undefined.                                                          |
| 42701      | The error code returned because the column is duplicate.                                                          |
| 42P03      | The error code returned because the cursor is duplicate.                                                          |
| 42P04      | The error code returned because the database is duplicate.                                                        |
| 42723      | The error code returned because the function is duplicate.                                                        |
| 42P05      | The error code returned because the prepared statement is duplicate.                                              |
| 42P06      | The error code returned because the schema is duplicate.                                                          |
| 42P07      | The error code returned because the table is duplicate.                                                           |
| 42712      | The error code returned because the alias is duplicate.                                                           |
| 42710      | The error code returned because the object is duplicate.                                                          |
| 42702      | The error code returned because no specific column is specified.                                                  |
| 42725      | The error code returned because no specific function is specified.                                                |
| 42P08      | The error code returned because no specific parameter is specified.                                               |
| 42P09      | The error code returned because no specific alias is specified.                                                   |
| 42P10      | The error code returned because the referenced column is invalid.                                                 |
| 42611      | The error code returned because the column definition is invalid.                                                 |
| 42P11      | The error code returned because the cursor definition is invalid.                                                 |
| 42P12      | The error code returned because the database definition is invalid.                                               |
| 42P13      | The error code returned because the function definition is invalid.                                               |
| 42P14      | The error code returned because the definition of a prepared statement is invalid.                                |
| 42P15      | The error code returned because the schema definition is invalid.                                                 |
| 42P16      | The error code returned because the table definition is invalid.                                                  |
| 42P17      | The error code returned because the object definition is invalid.                                                 |
| Class 44: WITH CHECK option violation.                                                                                        ||
| 44000      | The error code returned because the WITH CHECK option is violated.                                                |
| Class 53: insufficient resources.                                                                                             ||
| 53000      | The error code returned because the resources are insufficient.                                                   |
| 53100      | The error code returned because the disk is full.                                                                 |
| 53200      | The error code returned because the memory is exhausted.                                                          |
| 53300      | The error code returned because the number of connections exceeds the limit.                                      |
| Class 54: program limit exceeded.                                                                                             ||
| 54000      | The error code returned because the program limit is exceeded.                                                    |
| 54001      | The error code returned because the statement is complex.                                                         |
| 54011      | The error code returned because the number of columns exceeds the limit.                                          |
| 54023      | The error code returned because the number of arguments exceeds the limit.                                        |
| Class 55: object not in the required state.                                                                                   ||
| 55000      | The error code returned because the object is not in the required state.                                          |
| 55006      | The error code returned because the object is in use.                                                             |
| 55P02      | The error code returned because the runtime parameters cannot be modified.                                        |
| 55P03      | The error code returned because the lock is unavailable.                                                          |
| Class 57: operator intervention.                                                                                              ||
| 57000      | The error code returned because operator intervention occurs.                                                     |
| 57014      | The error code returned because the query is canceled.                                                            |
| 57P01      | The error code returned because the system is shut down by an administrator.                                      |
| 57P02      | The error code returned because the system is shut down when a crash occurs.                                      |
| 57P03      | The error code returned because the system cannot be connected.                                                   |
| Class 58: system error.                                                                                                       ||
| 58030      | The error code returned because an I/O error occurs.                                                              |
| 58P01      | The error code returned because the file is undefined.                                                            |
| 58P02      | The error code returned because the file is duplicate.                                                            |
| Class F0: configuration file error.                                                                                           ||
| F0000      | The error code returned because an error occurs in the configuration file.                                        |
| F0001      | The error code returned because a lock file exists.                                                               |
| Class P0: Procedural Language/PostgreSQL (PL/pgSQL) error.                                                                    ||
| P0000      | The error code returned because a PL/pgSQL error occurs.                                                          |
| P0001      | The error code returned because an exception is thrown.                                                           |
| Class XX: internal error.                                                                                                     ||
| XX000      | The error code returned because an internal error occurs.                                                         |
| XX001      | The error code returned because the data is corrupted.                                                            |
| XX002      | The error code returned because the index is corrupted.                                                           |



