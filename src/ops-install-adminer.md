

# Adminer

Adminer 是一个替换 phpMyAdmin 的数据库管理基面,可以用来导出、导入数据库、执行sql等操作，我们使用`docker run`快速安装


```bash
docker run -d --name=adminer \
--restart always \
--network mynet \
-p 8080:8080 \
adminer:4.8.0
```