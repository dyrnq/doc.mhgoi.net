# 创建数据库

## create-database

```sql
-- mysql5.7
CREATE DATABASE `mhgoi_blog` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
-- mysql8
CREATE DATABASE `mhgoi_blog` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci;

-- create user
CREATE USER 'jack'@'%' IDENTIFIED BY '666666';
GRANT ALL PRIVILEGES ON *.* TO 'jack'@'%';
FLUSH PRIVILEGES;
```

## mysql5.7-schema-and-data

| database-version | remark          | sql                                                                                       |
|------------------|-----------------|-------------------------------------------------------------------------------------------|
| mysql5.7         | only schema     | <https://github.com/dyrnq/mhgoi-blog-deploy/tree/main/sql/mysql/mysql-schema-mysql57.sql> |
| mysql5.7         | only data       | <https://github.com/dyrnq/mhgoi-blog-deploy/tree/main/sql/mysql/mysql-data-mysql57.sql>   |
| mysql5.7         | scheme and data | <https://github.com/dyrnq/mhgoi-blog-deploy/tree/main/sql/mysql/mysql-mysql57.sql>        |

## mysql8-schema-and-data

| database-version | remark          | sql                                                                                      |
|------------------|-----------------|------------------------------------------------------------------------------------------|
| mysql8           | only schema     | <https://github.com/dyrnq/mhgoi-blog-deploy/tree/main/sql/mysql/mysql-schema-mysql8.sql> |
| mysql8           | only data       | <https://github.com/dyrnq/mhgoi-blog-deploy/tree/main/sql/mysql/mysql-data-mysql8.sql>   |
| mysql8           | scheme and data | <https://github.com/dyrnq/mhgoi-blog-deploy/tree/main/sql/mysql/mysql-mysql8.sql>        |
