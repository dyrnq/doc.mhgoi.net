# postgresql

## postgresql-11.x

```bash
mkdir -p /data/lib/postgres11;
docker run -d --name postgres11 \
--restart always \
--network mynet \
-e POSTGRES_PASSWORD=666666 \
-p 5432:5432 \
-v /data/lib/postgres11:/var/lib/postgresql/data postgres:11.10
```

## postgresql-12.x

```bash
mkdir -p /data/lib/postgres12;
docker run -d --name postgres12 \
--restart always \
--network mynet \
-e POSTGRES_PASSWORD=666666 \
-p 5432:5432 \
-v /data/lib/postgres12:/var/lib/postgresql/data postgres:12.5
```

## postgresql-13.x

```bash
mkdir -p /data/lib/postgres13;
docker run -d --name postgres13 \
--restart always \
--network mynet \
-e POSTGRES_PASSWORD=666666 \
-p 5432:5432 \
-v /data/lib/postgres13:/var/lib/postgresql/data postgres:13.2
```
