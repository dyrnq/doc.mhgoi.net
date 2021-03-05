# redis

redis使用`docker run`快速安装

```bash
docker run -d --name=redis \
--restart always \
--network mynet \
-p 6379:6379 \
redis:5.0.10
```
