# 部署前端web

## 编译
根据需要修改vue.config.js的publicPath

```json
publicPath: '/',
```

```bash
cd mhgoi-blog/frontend/web

npm run build
```

## 发布
```bash
cp dist/* /usr/share/nginx/html/
```