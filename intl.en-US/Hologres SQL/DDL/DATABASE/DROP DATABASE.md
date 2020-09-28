---
keyword: [DROP DATABASE, Hologres]
---

# DROP DATABASE

You can execute the DROP DATABASE statement to delete a database. This topic describes how to use the DROP DATABASE statement.

## Limits

-   Only a superuser of a database or the database owner configured by a superuser can execute this statement to delete the database.
-   When you delete a database, all objects in the database are deleted.
-   After a database is deleted, you can no longer connect to it.

## Syntax

```
DROP DATABASE [ IF EXISTS ] db_name;
```

## Examples

In the following example, a database named mydb is deleted:

```
DROP DATABASE mydb;
```

