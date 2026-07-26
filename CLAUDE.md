# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

个人摄影作品展示网站（中文），纯静态单页应用。展示旅行/风光摄影作品，当前收录济南千佛山日落系列和趵突泉雪景系列。

- **部署地址**: https://happyphoto.site（Cloudflare Workers）
- **仓库**: https://github.com/DevineCheung/happyphoto-site

## 技术栈

- **语言**: 纯 HTML5 + CSS3 + Vanilla JavaScript（无框架、无构建工具）
- **字体**: Google Fonts — Noto Serif SC、Playfair Display、Quicksand、Zhi Mang Xing
- **图片格式**: WebP
- **部署平台**: Cloudflare Workers（通过 wrangler 部署静态文件）

## 项目结构

```
happyphoto-site/
├── index.html              # 主页面（~1371 行）
├── qianfo_sunset.html      # 千佛山日落相册子页面（~676 行）
├── baotu_spring_snow.html  # 趵突泉雪景相册子页面（~1026 行）
├── wrangler.jsonc          # Cloudflare Workers 配置
├── .gitignore
├── images/                 # WebP 图片资源
│   ├── 千佛山日落/         # 千佛山日落系列（9 张相册照片）
│   │   ├── 封面.webp       # 卡片封面
│   │   ├── 顶部照片.webp   # 页面 Hero 大图
│   │   └── 相册/           # 9 张照片（00_丁达尔 ~ 08_松）
│   └── 趵突泉雪景/         # 趵突泉雪景系列（34 张相册照片）
│       ├── 封面.webp       # 卡片封面
│       ├── 顶部照片.webp   # 页面 Hero 大图
│       └── 相册/           # 34 张照片（主题分组命名）
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

### index.html（主页面，~1371 行）

单页布局，从上到下依次为：

1. **全屏视频 Hero** — MP4 背景视频，附带自动播放降级处理
2. **固定导航栏** — 透明→滚动后毛玻璃效果（`.scrolled` 类）
3. **文字轮播** — 7 秒间隔切换摄影主题引语
4. **天空色谱（Sky Chroma）** — 120 色块交互展示，支持鼠标悬停提示、色块/渐变视图切换
5. **作品集网格** — 4 个卡片（2 个真实入口 + 2 个占位）
6. **Canvas 动画区** — HAPPY·PHOTO 标识动画（粒子、鼠标跟踪、闪烁笑脸）
7. **关于/简介**
8. **页脚** — 社交媒体链接（微信、微博、抖音、小红书）

### qianfo_sunset.html（千佛山日落，~676 行）

1. **加载屏** — 品牌标识 + 进度条
2. **精选大图 Hero** — 暗色主题（#0c0c0c），金色点缀（#c9a96e）
3. **相册网格** — 9 张照片，使用 Zhi Mang Xing 书法字体做标题装饰
4. **全屏 Lightbox** — 上一张/下一张导航、键盘快捷键（Escape / ArrowKeys）、点击外部关闭
5. **页面过渡动画** — 缩放 + 模糊叠加层

### baotu_spring_snow.html（趵突泉雪景，~1026 行）

1. **加载屏** — 品牌标识 + 进度条
2. **精选大图 Hero** — 暗色主题（#080c10），冰蓝色点缀（#8fbcd4）
3. **相册网格** — 34 张照片，根据主题分组展示（雪景、猫咪、金鱼、飞鸟等）
4. **全屏 Lightbox** — 上一张/下一张导航、键盘快捷键、触屏滑动支持、点击外部关闭
5. **页面过渡动画** — 缩放 + 模糊叠加层
6. **额外效果** — 滚动驱动的视差效果、照片淡入序列动画、雪花粒子 Canvas 动画

## 编码约定

### CSS
- 所有样式内联在 `<style>` 标签中，无外部样式表
- 每个页面独立设计：千佛山暗底金色调（#0c0c0c / #c9a96e），趵突泉暗底冰蓝调（#080c10 / #8fbcd4），首页浅底深字（#fafbfc / #1a2332）
- 响应式断点: 1024px / 768px / 480px
- 滚动触发动画通过 `IntersectionObserver` + CSS transition class 实现
- 导航栏 `:root` 仅设置 `color-scheme: light`

### JavaScript
- Vanilla JS，使用 IIFE 闭包，无模块/导入
- 页面入场/出场动画通过 CSS keyframes + JS 切换类控制
- 图片懒加载/滚动显现使用 `IntersectionObserver`

### 图片资源
- 图片统一使用 WebP 格式
- **千佛山日落** 命名规范: `序号_中文名.webp`（如 `01_晖.webp`），按展示顺序编号（00_丁达尔 ~ 08_松）
- **趵突泉雪景** 命名规范: 主题分组命名，单一照片用纯数字（`01.webp`），组照用 `序号_子序号`（`02_01.webp` ~ `02_03.webp`），主题照片用 `主题_序号`（`猫_01.webp`、`鸟_01_01.webp`）

### 字体
- 中文字体首选: Noto Serif SC → PingFang SC → Microsoft YaHei
- 英文字体: Playfair Display、Quicksand
- 相册页面额外使用 Zhi Mang Xing 书法字体做标题装饰

## 部署

通过 Cloudflare Workers 部署（`wrangler.jsonc` 配置），`assets.directory` 设为 `.`，即直接发布仓库根目录内容。

**重要**: 部署前无需构建步骤，修改 HTML 文件后直接 `wrangler deploy` 即可。
