

# mariadb

## mariadb-10.5.x

```bash
mkdir -p /data/lib/mariadb10;
docker run -d --name mariadb10 \
--restart always \
--network mynet \
-e MYSQL_ROOT_PASSWORD=666666 \
-v /data/lib/mariadb10:/var/lib/mysql \
-p 3306:3306 \
mariadb:10.5.8
```
