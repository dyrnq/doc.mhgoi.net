# Artalk评论组件

## Artalk简介

> 评论组件基于[https://github.com/ArtalkJS/Artalk](https://github.com/ArtalkJS/Artalk)

需要部署他的服务端[https://github.com/ArtalkJS/Artalk-API-PHP](https://github.com/ArtalkJS/Artalk-API-PHP)


```bash
# https://github.com/ArtalkJS/Artalk-API-PHP/blob/master/Config.example.php
wget --no-check-certificate --output-file Config.php https://raw.githubusercontent.com/ArtalkJS/Artalk-API-PHP/master/Config.example.php

mkdir $(pwd)/artalk-data;

docker run -d --name artalkserverphp \
--restart always \
--network mynet \
-v $(pwd)/Config.php:/ArtalkServerPhp/Config.php \
-v $(pwd)/artalk-data:/ArtalkServerPhp/data \
-p 23366:23366 dyrnq/artalkserverphp
```

## 参考
* [https://github.com/ArtalkJS/Artalk](https://github.com/ArtalkJS/Artalk)
* [https://github.com/ArtalkJS/Artalk-API-PHP](https://github.com/ArtalkJS/Artalk-API-PHP)
* [https://github.com/dyrnq/docker-ArtalkServerPhp](https://github.com/dyrnq/docker-ArtalkServerPhp)