# 部署前端admin

## 编译
根据需要修改vue.config.js的publicPath

```json
publicPath: '/wp-admin',
```

```bash
cd mhgoi-blog/frontend/admin

npm run build
```

## 发布
```bash
mkdir -p /usr/share/nginx/html/wp-admin
cp dist/* /usr/share/nginx/html/wp-admin
```