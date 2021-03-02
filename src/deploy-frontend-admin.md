# 部署前端admin

## 编译


```bash
cd mhgoi-blog/frontend/admin

npm run build
```

## 发布
```bash
mkdir -p /data/www/adminblog.mhgoi.net/html
cp dist/* /data/www/adminblog.mhgoi.net/html
```