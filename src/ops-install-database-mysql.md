# mysql

## mysql-5.7.x

```bash
mkdir -p /data/lib/mysql57;
docker run -d --name mysql57 \
--restart always \
--network mynet \
-e MYSQL_ROOT_PASSWORD=666666 \
-v /data/lib/mysql57:/var/lib/mysql \
-p 3306:3306 \
mysql:5.7.32 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci --default-time-zone=+8:00
```

## mysql-8

```bash
mkdir -p /data/lib/mysql8;
docker run -d --name mysql8 \
--restart always \
--network mynet \
-e MYSQL_ROOT_PASSWORD=666666 \
-v /data/lib/mysql8:/var/lib/mysql \
-p 13306:3306 \
mysql:8.0.23 --character-set-server=utf8mb4 --collation-server=utf8mb4_unicode_ci --default-time-zone=+8:00 --innodb-dedicated-server=on
```
