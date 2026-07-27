# 个人网站 — 项目概览

> 最后更新: 2026-07-27 22:27

---

## 项目结构

```
/Users/Andy/Desktop/个人网站/
├── index.html              ← 主页面（加载页 + Hero + 滚动视频 + 作品集 + 感谢页）
├── edit.html               ← Hero 文案可视化编辑器
├── video-edit.html         ← 视频文案可视化编辑器
├── SITE_OVERVIEW.md        ← 本文件
├── pic/
│   ├── 作品封面/        ← 7 张作品封面（均 WebP）
│   ├── 背景扩图.webp
│   ├── 背景.webp
│   ├── 线稿.webp
│   ├── 赛博身体.webp
│   ├── 前景.webp
│   ├── 跑步背景图.webp
│   └── 视频/
│       ├── 个人介绍页视频720.mp4     ← v1 原始 19.6MB
│       ├── 个人介绍页视频II720.mp4   ← v2 原始 17.2MB
│       ├── 个人介绍页视频2.mp4       ← v3 原始 13.5MB
│       ├── 个人介绍页视频III2K.mp4   ← v4 原始 2K 79.8MB
│       ├── intro-scroll.mp4          ← 旧版压缩 1.2MB（已弃用）
│       └── intro-scroll-kf.mp4       ← 当前使用 29.8MB（1080p 全关键帧）
├── backup/                 ← 当前版本备份
├── .workbuddy/
│   └── memory/             ← 工作日志归档
└── CNAME                   ← jonlab.cn
```

---

## 页面结构

### 加载页（全屏，所有内容之前）

CSS/SVG 赛博朋克风格加载页，`z-index: 1000`。图层（从底到顶）：

| 层 | 内容 | 说明 |
|----|------|------|
| 1 | wrapper 渐变底色 | `radial-gradient(#1a1a1a 0%, #000 100%)`，挡住 Hero 内容 |
| 2 | `pic/跑步背景图.webp` | 2752×1536, 87.7KB, opacity 0.5, 无模糊 |
| 3 | 径向渐变遮罩（::after） | 边缘 `rgba(0,0,0,0.7)` → 中心透明，暗角效果 |
| 4 | 旋转环 + SVG 进度圆环 + 文字 | neon 青色调 |

**交互**：
- 真实追踪帧加载进度 `window._frameLoadProgress`
- **80% 帧加载完即可退出**（`p >= 0.8`），剩余 20% 后台下载
- 0.6s 渐隐消失，20s 强制超时兜底

**跑酷风状态文字**（6 段对应真实进度）：
| 进度 | 文案 |
|------|------|
| 0% ~ 15% | 热身... |
| 15% ~ 35% | 助跑... |
| 35% ~ 55% | 起跳！ |
| 55% ~ 75% | 空中转体... |
| 75% ~ 99% | 稳稳落地... |
| 100% | ✓ 完美落地 |

### 第一屏 — Hero

| 层级 | 内容 |
|---|---|
| z-index -2 | `pic/背景扩图.webp` 全屏背景 |
| z-index -1 | 文案 "Hi there / I am Jon"（62px + 183px, 渐变文字） |
| z-index 0 | Unicorn Studio 3D 场景（Project: `Sduh8PTMFfeiFEU6lmXA`） |

**视差**：向下滚动时多层变速（背景 0.3x → 文案 0.55x/0.65x → 3D 场景 1.0x）

### 第二屏 — 滚动逐帧视频

| 层级 | 内容 |
|---|---|
| `<video>` | 全屏，`object-fit: cover`，直接渲染 |
| 文字叠加 | 5 段文案，随视频时间滚动浮现/消失（纯白 + 双层光晕） |
| 加载 spinner | `canplay` 后自动消失 |
| 进度条 | 底部 3px 白条 |

**技术细节**：
- 零 Canvas，直接 `<video>.currentTime` 硬件加速渲染
- 视频 `intro-scroll-kf.mp4`：3184×1792 2K 原片 → ffmpeg 缩至 1080p + `-g 1` 全关键帧 + `-crf 18` 视觉无损，**29.8MB**
- 时长自动检测 `video.duration`，换视频无需改代码
- 500vh 总高，400vh 滚动空间 → 15 秒慢速播放
- `prefers-reduced-motion` 用户自动静帧

**5 段文案**（最终定稿）：

| 段 | 时间 | 文字 | 位置 | 偏移 |
|---|------|------|------|------|
| 1 | 0→1.6s | 我是王壮壮，我有很多爱好…… | 居中 | -180px |
| 2 | 2→5.2s | 首先，是喝牛奶 / 才怪，我爸让我写的 😅 | 底部左对齐 | — |
| 3 | 6→8.9s | 最近，爱上了 AI 编程 / 做了很多好玩的事儿～ | 左侧 | +140px 下移 |
| 4 | 9.8→12.6s | 我还爱画画 / 随手画的那种🤣 | 左侧 | +25px, +105px |
| 5 | 13.9→15s | 跑酷，练了八年！ | 左侧 | +65px, -125px |

### 第三屏 — 作品集（"Our Best Mistakes" 卡片画廊）

复刻 halftone.aura.build 的 "our best mistakes" 风格（黄胶带 + 红字标签拍立得卡片）。**已替换原 Coverflow 焦点画廊**，旧 Coverflow 代码见 `backup/SITE_OVERVIEW.md`。

| 作品 | 封面 | 标签 | 标题（显示） | 链接 |
|---|---|---|---|---|
| 双灵坠 | `双铃坠封面 2.webp` | 动画短片 | 双灵坠 | [B站](https://www.bilibili.com/video/BV1JrQ9B1Ecp/) ↗ |
| 中国古代疆域地图 | `中国古代疆域.webp` | 数据可视化 | 中国古代疆域地图 | [3D地图](http://history.jonlab.cn/maplibre_3d_history.html) ↗ |
| 中国古代诗词地图 | `中国古代诗词地图.webp` | 数据可视化 | 中国古代诗词地图 | [诗词地图](https://history.jonlab.cn/maplibre_3d_history.html?mode=poem) ↗ |
| 玉米·黄金·美洲豹 | `玉米黄金美洲豹.webp` | 策略塔防游戏 | 玉米·黄金·美洲豹 | 暂无 |
| 人物设定图 | `人物设定图.webp` | 人物设计 | 人物设定图 | 暂无 |
| 近地轨道军事卫星设计图 | `卫星.webp` | 3D 建模 | 近地轨道军事卫星设计图 | 暂无 |
| 星际飞船设计图 | `飞船.webp` | 3D 建模 | 星际飞船设计图 | 暂无 |

- 纯黑底 `#000` + Unicorn Studio 动态背景（Project: `lEMBUE0ODtdPgc7dwRWz`，opacity 0.3）
- 7 卡 flex 横排换行居中（`.mistake-gallery`，`max-width:1180px`，`gap:64px 60px`）
- 暖米灰浅卡 `#e9e6df`（300px，圆角 16px）+ 4:3 照片轻暗角；黄胶带 `.mc-tape`（`top:-13px; left:27%; width:46%`，`rotate(-3deg)`）；右上红字黄底标签 `.mc-label`（`#ffe86e`/`#c23a52`，`right:-16px` 斜贴角）
- 散落感：每卡独立倾斜角 `--base`（`-7/5/-4/6/-3/4/-5°`）+ 标签旋转（全逆时针 `-14/-16/-9/-12/-15/-13/-11°`）
- 3D 鼠标跟随：JS 监听 `#mistakeGallery` 内 `mousemove` 设 `--rx/--ry`（±19° `*38`，perspective 700px）；hover 上浮 `translateY(-14px) scale(1.05)` 并 `z-index:100`
- 卡片 1–3 用 `<a class="mc-card-link">` 包裹可跳转（新窗口 + `rel="noopener noreferrer"`），4–7 纯展示

**⚠️ 改此区块必看**：
1. nth-child 分两套：1–3 写 `.mc-card-link:nth-child(N)`，4–7 写 `.mistake-gallery > .mc-card:nth-child(N)`，混用则角度/标签回退成统一基础值。
2. 裸卡 `--base` 规则勿多写 `.mc-card`（写成 `… :nth-child(4) .mc-card` 匹配不到，卡片仍平直）。
3. 标签须放 `.mc-photo` 外（照片 `overflow:hidden` 会裁掉），作 `.mc-card` 直接子元素。
4. 长标签 `right` 探出 + 旋转会超右缘，列距不足时右卡（DOM 靠后）会盖住；当前 `right:-16px` + 列距 `60px` + 字号 `12px` 已规避。
5. **`.portfolio` 勿用 `overflow-x: hidden`**（已踩坑）。CSS 规范规定：`overflow-x: hidden` + `overflow-y` 默认 `visible` 时，`visible` 视同为 `auto`→浏览器生成次级垂直滚动条。改用 `overflow: clip` 可裁切溢出且不触发滚动上下文。2026-07-27 用户报告双滚动条，根源即此。

### 第四屏 — 感谢页

纯 Unicorn Studio 3D 场景（Project: `HTkCrCAs4VhJld61Baso`），**无任何文案**。

---

## 滚动差速效果（视差与淡入）

**当前方案**：无整屏间位移差速（--sec-p 已移除）。作品集内部保有一层微妙的卡片比图片更快的差速，视频屏保留 opacity 淡入。

### 视频屏淡入（--sv-op）

| 屏 | 差速方式 | CSS 变量 | 范围 | 说明 |
|---|---|---|---|---|
| 第二屏 滚动视频 | opacity 淡入差速 | `--sv-op` | 0.6→1 | 一直保留。屏内 `<video>` 用 `position:sticky` 钉住，整体 translateY 会破坏 sticky 定位 → 不透明度差速不影响视频播放。JS 基于 `getBoundingClientRect` 相对视口进度驱动。 |

### 作品集内部卡差速（--card-p）

| 目标 | 差速方式 | CSS 变量 | 系数 | 说明 |
|---|---|---|---|---|
| 卡片（`.portfolio-inner`） | 比图片快 | `--card-p` | 1.6x | 图片跟随自然滚动无额外位移，卡片在整体位移基础上额外快移（p × 160 × 1.6，最大值约 256px），形成「卡片冲得比图片快」的内部层次感。 |

**历史**：曾有过 `--sec-p` 整屏位移方案（作品集+160px/感谢页-90px），但因 sticky 导致的视觉割裂和双滚动条问题，已移除。

### 实现要点
- 视频屏和内部卡差速共用同一个 IIFE，`requestAnimationFrame` 节流（passive scroll 监听），`resize` 重算。
- JS 只 `setProperty` 设 `--sv-op` / `--card-p`，绝不拼 transform 字符串 → 与卡片 3D 鼠标跟随（`--rx/--ry`）零冲突。
- CSS：`.portfolio-inner { transform: translateY(var(--card-p,0px)); will-change: transform; }`；`.scroll-video-sticky { opacity: var(--sv-op,1); will-change: opacity; }`

---

## 技术依赖

- **Unicorn Studio SDK v2.2.8** — self-loading snippet，CDN: `cdn.jsdelivr.net/gh/hiunicornstudio/unicornstudio.js@v2.2.8`
- **ffmpeg 7.1** — 视频重编码（全关键帧）
- 纯静态 HTML/CSS/JS，零框架依赖

## 相关工具

- `edit.html` — Hero 文案字号/颜色/偏移可视化编辑器
- `video-edit.html` — 视频 5 段文案时间线/内容/位置/偏移可视化编辑器

## 开发历程

| 日期 | 内容 |
|------|------|
| 7/25 | 加载页、Hero 视差、作品集 Coverflow 画廊 |
| 7/26 | 滚动视频播放、全关键帧编码、5 段文案系统、视频编辑器 |
| 7/27 | 2K→1080p 升级、编辑器 Bug 修复、文案定稿、文字效果优化、作品集替换为 Our Best Mistakes 卡片画廊 |
| 7/27 (晚) | 屏间滚动视差定稿：作品集(+80)/感谢页(-90) translateY 差速 + 视频屏 opacity 淡入差速(0.6~1)；Hero 不参与仅留内部多层视差；早期误做的板块内 5 层视差已清除 |
| 7/27 (补) | 作品集视差加强：±80px → ±160px（翻倍），其他屏不变 |
| 7/27 (晚修) | 作品集内部视差加卡片快于图片（--card-p 1.6x）；修复 sticky 双滚动条根因（overflow-x:hidden→overflow:clip）；--sec-p 整屏位移因双滚动条问题已整体移除；滚动差速章节重写 |
| 7/27 (3D) | 3D 卡片效果加强：视角 ±15°→±19°(×38)、perspective 800px→700px |
