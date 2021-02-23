# 安装nginx

> 参考  [http://nginx.org/en/docs/install.html](http://nginx.org/en/docs/install.html)

## debian
```bash
apt install nginx
```

## centos
```bash
yum install nginx
```

# 配置nginx

```conf

server{
    listen 80;
    server_name adminblog.mhgoi.net;
    location / {
        index index.html index.htm;
        root /data/www/adminblog.mhgoi.net/html;
        try_files $uri /index.html;
        if ($uri ~* ^.+\.(css|js|swf|png|woff|woff2|webp|svg|jgp)$) {
            access_log off;
            expires 30d;
        }

    }
    location /pic {
      alias  /data/pic/;
      index  index.html index.htm;
    }
    location /api {
      proxy_redirect off;
      proxy_http_version 1.1;
      proxy_set_header Connection "";
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_connect_timeout 10;
      proxy_send_timeout 10;
      proxy_read_timeout 10;
      proxy_buffer_size 4k;
      proxy_buffers 4 32k;
      proxy_busy_buffers_size 64k;
      proxy_temp_file_write_size 64k;
      proxy_ignore_client_abort on;
      proxy_pass http://mhgoi_backend/api;
    }
}


server{
    listen 80;
    server_name blog.mhgoi.net;
    client_max_body_size 75M;
    charset UTF-8;
    location / {
        index index.html index.htm;
        root /usr/share/nginx/html/;
        try_files $uri /index.html;
        if ($uri ~* ^.+\.(css|js|swf|png|woff|woff2|webp|svg|jgp)$) {
            access_log off;
            expires 30d;
        }
    }

    location /pic {
      alias  /data/pic/;
      index  index.html index.htm;
    }
    
    location /api {
      proxy_redirect off;
      proxy_http_version 1.1;
      proxy_set_header Connection "";
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_connect_timeout 10;
      proxy_send_timeout 10;
      proxy_read_timeout 10;
      proxy_buffer_size 4k;
      proxy_buffers 4 32k;
      proxy_busy_buffers_size 64k;
      proxy_temp_file_write_size 64k;
      proxy_ignore_client_abort on;
      proxy_pass http://mhgoi_backend/api;
    }

    location /ArtalkServer {
      proxy_redirect off;
      proxy_http_version 1.1;
      proxy_set_header Connection "";
      proxy_set_header Host $host;
      proxy_set_header X-Real-IP $remote_addr;
      proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
      proxy_connect_timeout 10;
      proxy_send_timeout 10;
      proxy_read_timeout 10;
      proxy_buffer_size 4k;
      proxy_buffers 4 32k;
      proxy_busy_buffers_size 64k;
      proxy_temp_file_write_size 64k;
      proxy_ignore_client_abort on;
      proxy_pass http://artalkserverphp;
    }
}
upstream mhgoi_backend {
    server   127.0.0.1:8001  weight=1 max_fails=2 fail_timeout=10s;
    keepalive 300;
}
upstream artalkserverphp {
    server   127.0.0.1:23366  weight=1 max_fails=2 fail_timeout=10s;
    keepalive 300;
}

```