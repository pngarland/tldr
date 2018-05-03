# mysql

> The MySQL command-line tool.

- Connect to a database:

`mysql {{database_name}}`

- Connect to a database, user will be prompted for a password:

`mysql -u {{user}} --password {{database_name}}`

- Connect to a database on another host:

`mysql -h {{database_host}} {{database_name}}`

- Connect to a database through a Unix socket:

`mysql --socket {{path/to/socket.sock}}`

- Execute SQL statements in a script file (batch file):

`mysql {{database_name}} < {{script.sql}}`

- Conditionally write field to a table:

```sql
SET @dbname = DATABASE();
SET @tablename = "posts";
SET @columnname = "new_column";
SET @preparedStatement = (SELECT IF(
(
    SELECT COUNT( * ) FROM INFORMATION_SCHEMA.COLUMNS WHERE(table_name = @tablename) AND(table_schema = @dbname) AND(column_name = @columnname)
) > 0,
"SELECT 1",
CONCAT("ALTER TABLE ", @tablename, " ADD ", @columnname, " VARCHAR(15);")
));
PREPARE alterIfNotExists FROM @preparedStatement;
EXECUTE alterIfNotExists;
DEALLOCATE PREPARE alterIfNotExists;
```
