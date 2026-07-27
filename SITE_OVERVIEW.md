# 个人网站 — 项目概览

> 最后更新: 2026-07-26 02:12

---

## 项目结构

```
/Users/Andy/Desktop/个人网站/
├── index.html              ← 主页面
├── edit.html               ← 可视化文案编辑器
├── SITE_OVERVIEW.md        ← 本文件
├── pic/
│   ├── 作品封面/
│   │   ├── 双铃坠封面 2.png
│   │   ├── 中国古代疆域.png
│   │   └── 玉米黄金美洲豹.png
│   ├── 背景扩图.webp
│   ├── 背景.webp
│   ├── 线稿.webp
│   ├── 赛博身体.webp
│   └── 前景.webp
│   └── 视频/
│       ├── 个人介绍页视频720.mp4     ← 旧版原始（19.6MB）
│       ├── 个人介绍页视频II720.mp4   ← 新版原始（17.2MB）
│       ├── intro-scroll.mp4          ← 旧版压缩（1.2MB，已弃用）
│       └── intro-scroll-kf.mp4       ← 当前使用（15.6MB，全关键帧）
├── backup/                 ← 当前版本备份
│   ├── index.html
│   ├── edit.html
│   └── SITE_OVERVIEW.md
└── .workbuddy/
```

---

## 页面结构

### 第一屏 — Hero

从上到下层叠顺序（z-index 从低到高）：

| z-index | 内容 | 说明 |
|---|---|---|
| -2 | `pic/背景扩图.webp` | 全屏背景图 |
| -1 | 文案 "Hi there / I am Jon" | 两行白色粗体文字 |
| 0 | Unicorn Studio WebGL 3D 场景 | Project ID: `Sduh8PTMFfeiFEU6lmXA`，SDK v2.2.8 |
| 0 | Loading 占位 spinner | 场景渲染后自动消失 |

### 🎬 视差效果
向下滚动时多层变速，制造纵深推进感：

| 层级 | 滚动速度 | 技术实现 |
|---|---|---|
| 背景图 (背景扩图.webp) | 0.3x 🐢 最慢 | JS requestAnimationFrame + translateY |
| 文案 "Hi there" | 0.55x | JS requestAnimationFrame + translateY（慢于第二行） |
| 文案 "I am Jon" | 0.65x | JS requestAnimationFrame + translateY（快于第一行） |
| 3D 场景 (Unicorn Studio) | 1.0x 🚀 正常 | 自然滚动，无干预 |

### 文案样式（已定稿）

```css
.hero-bg-text {
  justify-content: center;
  /* translateY 由 JS 视差控制，初始偏移 -107px */
}

.line-1 /* "Hi there" */ {
  font-size: 62px;
  background: linear-gradient(180deg, #ffffff 0%, rgba(255,255,255,0.3) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  transform: translateX(-362px);  /* 左移 */
  margin-bottom: 0px;
}

.line-2 /* "I am Jon" */ {
  font-size: 183px;
  background: linear-gradient(180deg, #ffffff 0%, rgba(255,255,255,0.3) 100%);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
  transform: translateX(-146px);  /* 左移 */
}
```

> 文案采用 Apple 发布会风格的上亮下暗渐变效果。

### 第二屏 — 滚动逐帧视频

| 层级 | 内容 |
|---|---|
| `<video>` | 全屏 `<video>` 元素，`object-fit: cover` 裁剪，直接渲染 |
| 加载 spinner | 视频加载中显示，`canplay` 后自动消失 |
| 进度条 | 底部 3px 白条，随滚动实时填充 |

**交互**：进入该区域后，视频画面固定（`position: sticky; top: 0`），用户滚动 ≈ 播放进度条。**500vh 总区域，400vh 滚动空间 = 15 秒视频慢速播放**（约 2 倍于正常滚动速度）。

**技术细节**：
- 零 Canvas 开销，直接操作 `<video>.currentTime`，浏览器硬件加速渲染
- 视频 `intro-scroll-kf.mp4` 用 ffmpeg 以 `-g 1 -keyint_min 1` 编码，**361 帧全是关键帧**，滚动到任意位置瞬间解码无需等待
- 画质：`-crf 18`（视觉无损）+ `-preset veryslow`，15.6MB 比原版 17.2MB 还小
- 时长自动检测 `video.duration`，换视频无需改代码
- `prefers-reduced-motion` 用户自动静帧（由浏览器原生处理）

**视频文件清单**：
| 文件 | 大小 | 用途 |
|------|------|------|
| `个人介绍页视频II720.mp4` | 17.2 MB | 原始文件，不做压缩 |
| `intro-scroll-kf.mp4` | 15.6 MB | **当前使用**，全关键帧编码 |
| `intro-scroll.mp4` | 1.2 MB | 旧版 WiFi 预设压缩（已弃用） |
| `个人介绍页视频720.mp4` | 19.6 MB | 第一版原始文件 |

### 第四屏 — 感谢页

| 层级 | 内容 |
|---|---|
| z-index 0 | Unicorn Studio WebGL 3D 场景（Project: `b4yDz9Fg5aupJU3GGOTp`，SDK v2.2.8） |
| z-index 0 | Loading 占位 spinner（场景渲染后自动消失） |

- 纯 3D 场景，**无任何文案**
- 复用 SDK 自加载脚本，无需额外引入

---

### 第三屏 — 作品集（焦点画廊 Coverflow）

| 作品 | 封面 | 链接 |
|---|---|---|
| **双铃坠** | `作品封面/双铃坠封面 2.webp` | [B站视频](https://www.bilibili.com/video/BV1JrQ9B1Ecp/) ↗ |
| **中国古代疆域** | `作品封面/中国古代疆域.webp` | [3D地图](http://history.jonlab.cn/maplibre_3d_history.html) ↗ |
| **中国古代诗词地图** | `作品封面/中国古代诗词地图.webp` | [诗词地图](https://history.jonlab.cn/maplibre_3d_history.html?mode=poem) ↗ |
| **玉米·黄金·美洲豹** | `作品封面/玉米黄金美洲豹.webp` | 暂无 |
| **人物设定** | `作品封面/人物设定图.webp` | 暂无 |
| **军事卫星设计图** | `作品封面/卫星.webp` | 暂无 |
| **星际飞船设计图** | `作品封面/飞船.webp` | 暂无 |

- 纯黑背景 `#000` + Unicorn Studio WebGL 3D 场景作为动态背景（Project: `lEMBUE0ODtdPgc7dwRWz`，opacity 0.3，`data-us-production`）
- Coverflow 焦点画廊：所有作品排成一排同时可见，中间最大、两侧渐小，持续缓慢向左漂移（rAF 驱动，~33 秒一圈）
- 纯 2D 变换（`translateX + scale`），无 rotateY
- 交互：左右箭头 / 点击两侧卡片 / 键盘 ← → / 底部指示点切换；首尾循环（取模归一化）
- **点击两侧卡片不是瞬间跳转，而是快速平滑滑到中间**（指数衰减，`diff * 0.15`/帧，约 0.3 秒完成）
- **鼠标悬停在任意作品上暂停漂移**，移出恢复（移动端触摸同理）
- 仅中间（active）作品可点击跳转，两侧点击仅切换居中
- 响应式：`gap = cardWidth * 0.95` 保证不重叠，移动端自适应
- 封面图已缩放至宽 880px 并转 WebP（保留透明），原 PNG 作源文件保留在 `pic/作品封面/`
- ⚠️ **水印**：Unicorn Studio 免费版会在场景渲染"made with unicorn.studio"水印，`data-us-production` 和 CSS 均无法移除，需付费许可去除

---

## 技术依赖

- **Unicorn Studio SDK v2.2.8** — 通过 self-loading snippet 动态加载
- **CDN**: `cdn.jsdelivr.net/gh/hiunicornstudio/unicornstudio.js@v2.2.8`
- 纯静态 HTML/CSS/JS，无框架依赖

---

## 相关工具

- `edit.html` — 可视化编辑器，可在浏览器中拖拽调整两行文案的字号、颜色、横向/纵向偏移
- 编辑器中调整满意后点击「生成 CSS 代码」，将输出值替换到 `index.html` 对应位置
