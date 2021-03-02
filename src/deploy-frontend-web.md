# 部署前端web

## 编译


```bash
cd mhgoi-blog/frontend/web

npm run build
```

## 发布
```bash
mkdir -p /data/www/blog.mhgoi.net/html
cp dist/* /data/www/blog.mhgoi.net/html
```