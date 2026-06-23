# 班级积分 · 部署指南

## 快速启动（本地预览）

### 方式一：Python 简易服务器

```bash
cd class-points-site
python3 -m http.server 8080
# 浏览器打开 http://localhost:8080
```

### 方式二：Node.js 服务器

```bash
cd class-points-site
npx serve .
# 浏览器打开 http://localhost:3000
```

### 方式三：Docker

```bash
docker run -d -p 8080:80 -v $(pwd):/usr/share/nginx/html:ro nginx:alpine
# 浏览器打开 http://localhost:8080
```

---

## 部署到生产环境

### Vercel（推荐，免费）

1. 安装 Vercel CLI：`npm i -g vercel`
2. 在项目目录执行：`vercel --prod`
3. 或将 GitHub 仓库连接到 vercel.com 自动部署

### Netlify（推荐，免费）

1. 将项目推送到 GitHub
2. 在 netlify.com 连接该仓库
3. 发布目录设为 `class-points-site`
4. 无需构建命令

### 自有服务器（Nginx）

```nginx
server {
    listen 80;
    server_name your-domain.com;
    root /path/to/class-points-site;
    index index.html;
    location / {
        try_files $uri $uri/ /index.html;
    }
}
```

### 群晖 NAS

将整个 `class-points-site` 文件夹放入 Web Station 的虚拟主机目录即可。

---

## 定制修改指南

### 修改网站名称和描述

编辑 `index.html` 中的 `<title>` 和 `<meta name="description">`。

### 更换配色主题

编辑 `assets/index.css` 文件开头的 CSS 变量：

```css
/* ===== 设计令牌 - 修改此处可换肤 ===== */
:root {
  --bg-canvas: #fafaf9;         /* 背景色 */
  --brand-primary: #0f766e;     /* 品牌主色（青绿）*/
  --accent: #0891b2;            /* 强调色（蓝色）*/
  --success: #15803d;           /* 成功色 */
  --warning: #b45309;           /* 警告色 */
  --danger: #b91c1c;            /* 危险色 */
  ...
}
```

### 深色模式

编辑 `assets/index.css` 中的 `[data-theme=dark]` 区块。

### 修改文案（中文字符串）

在 `assets/index.js` 中搜索对应中文直接替换。常用文案示例：

| 原文 | 可替换为 |
|---|---|
| 班级 | 团队/社群/小组 |
| 班主任 | 管理员 |
| 学生 | 成员/学员 |
| 全班学生 | 全体成员 |
| 积分 | 学分/经验值/金币 |

---

## 技术架构

- **框架**: React 18
- **构建**: Vite
- **图表**: ECharts
- **表格**: SheetJS (xlsx)
- **样式**: Tailwind CSS v4
- **存储**: 浏览器 localStorage（数据不上传服务器）
- **部署**: 纯静态文件，无需后端

---

## 注意事项

1. 数据存储在浏览器本地，清除浏览器数据会丢失所有信息
2. 如需多设备同步，需自行开发后端（参考 README.md）
3. 大屏展示模式依赖屏幕分辨率，建议 1920×1080 以上
4. 打印功能使用 CSS `@media print`，可在打印预览中调整纸张方向为"横向"
