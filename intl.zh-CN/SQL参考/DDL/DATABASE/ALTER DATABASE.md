---
keyword: [ALTER DATABASE, Hologres]
---

# ALTER DATABASE

ALTER DATABASE语句用于修改数据库。本文为您介绍ALTER DATABASE的用法。

## 语法

```
ALTER DATABASE <dbname> SET configuration_parameter { TO | = } { value | DEFAULT }
ALTER DATABASE <dbname> SET configuration_parameter FROM CURRENT
ALTER DATABASE <dbname> RESET configuration_parameter
ALTER DATABASE <dbname> RESET ALL
```

参数说明如下表所示。

|参数|描述|
|--|--|
|configuration\_parameter|Hologres的配置参数。|
|value|配置参数的取值。|
|DEFAULT|设置配置参数为缺省值。|
|RESET|重置指定配置参数。|
|RESET ALL|重置数据库的所有配置参数。|
|SET FROM CURRENT|设置配置参数为当前Session的配置值。|

## 示例

设置时区为晚格林威治标准时间（北京时区）8个小时的时区，SQL语句如下。

```
ALTER DATABASE postgres SET timezone='GMT+8:00';
```

