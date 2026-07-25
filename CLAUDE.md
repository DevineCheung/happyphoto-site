# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

个人摄影作品展示网站（中文），纯静态单页应用。展示旅行/风光摄影作品，当前主要收录济南千佛山日落系列。

- **部署地址**: https://happyphoto.site（Cloudflare Workers）
- **仓库**: https://github.com/DevineCheung/happyphoto-site

## 技术栈

- **语言**: 纯 HTML5 + CSS3 + Vanilla JavaScript（无框架、无构建工具）
- **字体**: Google Fonts — Noto Serif SC、Playfair Display、Quicksand
- **图片格式**: WebP
- **部署平台**: Cloudflare Workers（通过 wrangler 部署静态文件）

## 项目结构

```
happyphoto-site/
├── index.html              # 主页面（单页，~1350行）
├── qianfo_sunset.html      # 千佛山日落相册子页面（~660行）
├── wrangler.jsonc          # Cloudflare Workers 配置
├── .gitignore
├── images/                 # WebP 图片资源
│   ├── 千佛山日落/         # 千佛山日落系列（按卡片组织）
│   │   ├── 封面.webp       # 卡片封面
│   │   ├── 顶部照片.webp   # 页面 Hero 大图
│   │   └── 相册/           # 画廊网格照片（按展示顺序命名）
│   └── 趵突泉雪景/         # 趵突泉雪景系列
│       ├── 封面.webp       # 卡片封面
│       ├── 顶部照片.webp   # 页面 Hero 大图
│       └── 相册/           # 全部37张照片（保留原始分组命名，中1=全宽）
└── videos/
    └── flower_background.mp4  # 首页视频背景
```

## 常用命令

| 用途 | 命令 |
|------|------|
| 本地预览 | `wrangler dev --port 8787` |
| 部署到生产 | `wrangler deploy` |
| 查看部署状态 | `wrangler deploy --dry-run` |

**注意**: 本工程无包管理器、无构建步骤，修改后直接部署即可。

## 页面结构

### index.html（主页面）

单页布局，从上到下依次为：

1. **全屏视频 Hero** — MP4 背景视频，附带自动播放降级处理
2. **固定导航栏** — 透明→滚动后毛玻璃效果（`.scrolled` 类）
3. **文字轮播** — 7 秒间隔切换摄影主题引语
4. **天空色谱（Sky Chroma）** — 120 色块交互展示，支持鼠标悬停提示、色块/渐变视图切换
5. **作品集网格** — 4 个卡片（1 个真实入口 + 3 个占位）
6. **Canvas 动画区** — HAPPY·PHOTO 标识动画（粒子、鼠标跟踪、闪烁笑脸）
7. **关于/简介**
8. **页脚** — 社交媒体链接（微信、微博、抖音、小红书）

### qianfo_sunset.html（相册子页面）

1. **加载屏** — 品牌标识 + 进度条
2. **精选大图 Hero**
3. **相册网格** — 10 张照片，根据宽高比动态分配 span 类（full/half/third）
4. **全屏 Lightbox** — 上一张/下一张导航、键盘快捷键（Escape / ArrowKeys）、点击外部关闭
5. **页面过渡动画** — 缩放 + 模糊叠加层

## 编码约定

### CSS
- 所有样式内联在 `<style>` 标签中，无外部样式表
- 响应式断点: 1024px / 768px / 480px
- 滚动触发动画通过 `IntersectionObserver` + CSS transition class 实现
- 导航栏 `:root` 仅设置 `color-scheme: light`

### JavaScript
- Vanilla JS，使用 IIFE 闭包，无模块/导入
- 页面入场/出场动画通过 CSS keyframes + JS 切换类控制
- 图片懒加载/滚动显现使用 `IntersectionObserver`

### 图片资源
- 图片统一使用 WebP 格式
- 新图片命名规范: `序号_中文名.webp`（如 `01_晖.webp`），按展示顺序编号

### 字体
- 中文字体首选: Noto Serif SC → PingFang SC → Microsoft YaHei
- 英文字体: Playfair Display、Quicksand

## 部署

通过 Cloudflare Workers 部署（`wrangler.jsonc` 配置），`assets.directory` 设为 `.`，即直接发布仓库根目录内容。

**重要**: 部署前无需构建步骤，修改 HTML 文件后直接 `wrangler deploy` 即可。
