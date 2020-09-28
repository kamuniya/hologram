---
keyword: [ALTER DATABASE, Hologres]
---

# ALTER DATABASE

You can execute the ALTER DATABASE statement to modify a database. This topic describes how to use the ALTER DATABASE statement.

## Syntax

```
ALTER DATABASE <dbname> SET configuration_parameter { TO | = } { value | DEFAULT }
ALTER DATABASE <dbname> SET configuration_parameter FROM CURRENT
ALTER DATABASE <dbname> RESET configuration_parameter
ALTER DATABASE <dbname> RESET ALL
```

The following table describes the parameters in the statement.

|Parameter|Description|
|---------|-----------|
|configuration\_parameter|The parameter to be modified.|
|value|The value of the parameter after the modification.|
|DEFAULT|Specifies to set the parameter to the default value.|
|RESET|Specifies to reset the parameter.|
|RESET ALL|Specifies to reset all the parameters of the database.|
|SET FROM CURRENT|Specifies to save the value of the parameter in the current session as the database-specific value.|

## Examples

In the following example, the time zone of a database is set to UTC+08:00, which is 8 hours ahead of Greenwich Mean Time \(GMT\):

```
ALTER DATABASE postgres SET timezone='GMT+8:00';
```

