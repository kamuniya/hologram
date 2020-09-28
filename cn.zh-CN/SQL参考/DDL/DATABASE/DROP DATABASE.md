---
keyword: [删除数据库, Hologres]
---

# DROP DATABASE

DROP DATABASE语句用于删除数据库。本文为您介绍DROP DATABASE的用法。

## 使用限制

-   只有数据库的Superuser，或被Superuser授权为数据库Owner的用户，才可以删除该数据库。
-   删除数据库时会同时删除数据库中的所有对象。
-   您不能再连接使用已删除的数据库。

## 语法

```
DROP DATABASE [ IF EXISTS ] db_name;
```

## 示例

删除数据库，示例语句如下。

```
DROP DATABASE mydb;
```

