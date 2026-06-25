# 集信智能官方网站

基于 **Astro** 构建的静态企业官网，集成 **Decap CMS** 可视化内容管理，支持 GitHub + Vercel 自动部署。

## 目录

- [技术栈](#技术栈)
- [项目结构](#项目结构)
- [快速开始](#快速开始)
- [部署流程](#部署流程)
- [可视化后台使用](#可视化后台使用)
- [SEO 优化说明](#seo-优化说明)
- [内容管理](#内容管理)

## 技术栈

| 技术 | 用途 |
|------|------|
| **Astro** | 静态站点生成器 (SSG) - 极速构建，SEO 友好 |
| **Decap CMS** | Git-based 可视化内容管理系统（原 Netlify CMS） |
| **MDX/Markdown** | 内容存储格式 - AI 爬虫易抓取 |
| **JSON-LD** | 结构化数据标记 - 提升搜索引擎/AI 理解 |
| **Sitemap** | 自动生成站点地图 |
| **Vercel** | 部署平台 - 自动从 GitHub 部署 |

## 项目结构

```
├── src/
│   ├── components/       # Astro 组件
│   │   ├── Header.astro  # 导航栏
│   │   └── Footer.astro  # 页脚
│   ├── layouts/
│   │   └── Layout.astro  # 全局布局 (SEO meta、样式)
│   ├── pages/
│   │   ├── index.astro       # 首页
│   │   ├── about.astro       # 关于我们
│   │   ├── products.astro    # 产品服务
│   │   ├── blog.astro        # 博客列表
│   │   ├── blog/[...slug].astro  # 博客详情
│   │   ├── contact.astro     # 联系我们
│   │   ├── privacy.astro     # 隐私政策
│   │   ├── terms.astro       # 服务条款
│   │   └── admin/index.astro # Decap CMS 后台入口
│   └── content/
│       ├── config.ts        # 内容集合配置
│       └── blog/            # 博客 Markdown 文件
├── public/
│   ├── admin/config.yml     # Decap CMS 配置
│   ├── robots.txt           # 爬虫规则 (AI 友好)
│   └── favicon.svg
├── astro.config.mjs         # Astro 配置
├── vercel.json              # Vercel 配置
└── package.json
```

## 快速开始

```bash
# 安装依赖
npm install

# 本地开发
npm run dev

# 构建
npm run build

# 预览构建结果
npm run preview
```

## 部署流程

### 第一步：推送到 GitHub

```bash
# 初始化仓库（如果尚未关联）
git init
git add .
git commit -m "Initial commit: 集信智能官网"

# 关联远程仓库（替换为你的仓库地址）
git remote add origin https://github.com/你的用户名/jixinzn-website.git
git branch -M main
git push -u origin main
```

### 第二步：在 Vercel 上部署

1. 访问 [vercel.com](https://vercel.com) → Add New → Project
2. 导入你的 GitHub 仓库 `jixinzn-website`
3. 框架会自动识别为 **Astro**（也可以手动选择）
4. 环境变量**无需额外配置**
5. 点击 Deploy

> Vercel 会自动识别 `vercel.json` 和 `astro.config.mjs`，无需额外设置。

### 第三步：配置自定义域名

在 Vercel 项目设置中：
1. Domains → 添加 `www.jixinzn.cn`
2. 根据提示在 DNS 管理中添加 CNAME 记录指向 `cname.vercel-dns.com`
3. 等待 SSL 证书自动签发

### 第四步：开启自动部署

默认情况下，每次推送到 `main` 分支，Vercel 会自动：
1. 拉取最新代码
2. 运行 `npm install`
3. 运行 `npm run build`
4. 部署到生产环境（秒级热更新）

## 可视化后台使用

### 访问后台

部署完成后，访问 `https://www.jixinzn.cn/admin/` 进入管理后台。

### 设置 GitHub OAuth (推荐)

要使用 Decap CMS，需要配置 GitHub OAuth 认证：

1. **创建 GitHub OAuth App**
   - 访问 GitHub Settings → Developer settings → OAuth Apps → New OAuth App
   - **Application name**: 集信智能 CMS
   - **Homepage URL**: `https://www.jixinzn.cn`
   - **Authorization callback URL**: `https://www.jixinzn.cn/admin/`
   - 生成 Client ID 和 Client Secret

2. **部署 OAuth 代理 (一键部署)**
   - 使用下面的模板一键部署到 Vercel：
   - [decap-cms-github-oauth-provider](https://github.com/vencruz/decap-cms-github-oauth-provider)
   - 配置 `GITHUB_CLIENT_ID` 和 `GITHUB_CLIENT_SECRET` 环境变量

3. **更新 `public/admin/config.yml`**
   - 将 `YOUR_GITHUB_USERNAME/jixinzn-website` 替换为你的实际仓库地址
   - 根据需要调整 OAuth 端点地址

### 在后台编辑内容

登录后，你可以在后台：

- **博客文章**：创建、编辑、发布 AI 行业洞察文章
- **页面内容**：编辑首页、关于我们等页面的文本内容
- **产品内容**：更新产品功能和描述
- **网站设置**：修改联系信息、网站标题等

每次保存，Decap CMS 会：
1. 自动提交更改到 GitHub 仓库
2. 触发 Vercel 自动重新部署
3. 网站内容在几秒内更新

## SEO 优化说明

### 已被 AI 爬虫优化的配置

| 优化项 | 实现方式 |
|--------|----------|
| **语义化 HTML** | Astro 生成标准 HTML5 结构 |
| **结构化数据** | 首页嵌入 JSON-LD (Organization Schema) |
| **Open Graph** | 所有页面包含 og:title, og:description, og:image |
| **Twitter Card** | 支持 Twitter 分享卡片 |
| **规范链接** | 每页有 canonical URL |
| **站点地图** | 自动生成 sitemap.xml |
| **爬虫规则** | robots.txt 允许所有主流 AI 爬虫 |
| **静态 HTML** | 纯静态页面，爬虫零门槛抓取 |
| **中文友好** | 使用中文语义标签和数据结构 |

### robots.txt 支持的 AI 爬虫

- GPTBot (OpenAI)
- Google-Extended
- CCBot (Common Crawl)
- anthropic-ai (Claude)
- Claude-Web
- PerplexityBot
- 以及其他标准搜索引擎

### 内容被 AI 收录的逻辑

由于所有内容都是**纯静态 Markdown/HTML**，AI 爬虫可以：

1. 通过 `/sitemap-index.xml` 发现所有页面
2. 直接抓取 HTML 内容（无需 JavaScript 渲染）
3. 解析 JSON-LD 结构化数据理解站点信息
4. 通过语义化 HTML 标签理解内容结构

## 内容管理

### 直接编辑 Markdown

所有内容存储在 `src/content/` 目录下，可以用任何编辑器直接编辑：

```markdown
---
title: "你的文章标题"
description: "SEO 描述"
pubDate: 2025-06-15
tags: ["标签1", "标签2"]
draft: false
---

这里是文章正文，支持 **Markdown** 格式。
```

### 通过 Decap CMS 后台编辑

非技术人员可以通过 `https://www.jixinzn.cn/admin/` 的可视化编辑器管理内容，无需了解代码。

## 许可证

MIT
