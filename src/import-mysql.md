# 创建数据库
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


