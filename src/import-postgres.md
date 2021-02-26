# 创建数据库

```sql
-- create database
create database mhgoi_blog with encoding='utf8';

-- create user
REVOKE ALL PRIVILEGES ON database mhgoi_blog FROM jack;
drop user jack;

create user jack with password '666666';
GRANT ALL PRIVILEGES on database mhgoi_blog to jack;

-- switch to your database and exec The following SQL

GRANT ALL PRIVILEGES ON ALL TABLES IN SCHEMA public TO jack;
GRANT ALL PRIVILEGES ON ALL SEQUENCES IN SCHEMA public to jack;
GRANT ALL PRIVILEGES ON ALL FUNCTIONS IN SCHEMA public to jack;

```


| database-version | remark          | sql                                                                                          |
|------------------|-----------------|----------------------------------------------------------------------------------------------|
| postgres11       | only schema     | <https://github.com/dyrnq/mhgoi-blog-deploy/tree/main/sql/postgres/postgres-schema-pg11.sql> |
| postgres11       | only data       | <https://github.com/dyrnq/mhgoi-blog-deploy/tree/main/sql/postgres/postgres-data-pg11.sql>   |
| postgres11       | scheme and data | <https://github.com/dyrnq/mhgoi-blog-deploy/tree/main/sql/postgres/postgres-pg11.sql>        |


| database-version | remark          | sql                                                                                          |
|------------------|-----------------|----------------------------------------------------------------------------------------------|
| postgres12       | only schema     | <https://github.com/dyrnq/mhgoi-blog-deploy/tree/main/sql/postgres/postgres-schema-pg12.sql> |
| postgres12       | only data       | <https://github.com/dyrnq/mhgoi-blog-deploy/tree/main/sql/postgres/postgres-data-pg12.sql>   |
| postgres12       | scheme and data | <https://github.com/dyrnq/mhgoi-blog-deploy/tree/main/sql/postgres/postgres-pg12.sql>        |
