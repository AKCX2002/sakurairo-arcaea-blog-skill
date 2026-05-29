---
name: sakurairo-arcaea-blog-skill
description: 应用 Arcaea/音游玻璃拟态风格 Sakurairo WordPress 博客的组合技能。封装了多轮迭代设计的精确 CSS 数值、配色 Token、层级体系、Sakurairo 主题冲突覆盖方案和 WordPress MCP 发布工作流。一个文件解决全部。
version: 1.7.0
---

# Sakurairo Arcaea Blog Skill

## 目的

在 Sakurairo v3.0.10 主题的 WordPress 博客 babel36acl.xyz 上，应用 Arcaea 风格（玻璃拟态、深色卡片层级、冰蓝配色）的页面设计、美化和发布。一个文件包含完整工作流。

## 何时使用

- 用户要求在 babel36acl.xyz 上"美化页面"、"应用 Arcaea 风格"、"设计 xx 页面"
- 需要创建带玻璃卡片层级的新页面（Games、Music 风格）
- 需要保持跨页面一致的 Arcaea 视觉语言
- 出现 Sakurairo 主题 CSS 冲突（blockquote、FontAwesome 图标等）
- 需要将文章/页面发布到 WordPress

---

## 目录

- [前置技能加载](#前置技能加载必须)
- [CSS 设计体系](#css-设计体系)
- [Sakurairo 主题集成](#sakurairo-主题集成)
- [WordPress 发布工作流](#wordpress-发布工作流)
- [工作流](#工作流)
- [Pitfalls](#pitfalls)

---

## 前置技能加载（必须！）

此技能被触发时，必须先按顺序加载以下技能。**注意**：部分技能位于 `/root/.agents/skills/` 目录而非 Hermes 内置技能库，需通过 read_file 直接读取。

### 核心技能（可从 .agents/skills 加载）

1. **`sakurairo-theme`** — Sakurairo 主题功能完整指南：短代码、颜色系统、AI 摘要等
2. **`glassmorphism-ui`** — 毛玻璃 CSS 模式库
3. **`ui-beautify`** — Arcaea v5 精确 CSS 数值
4. **`ui-designer`** — 整页布局生成
5. **`css-master`** — 设计 Token 体系、Sakurairo 冲突诊断

### Hermes 内置技能

6. **`wordpress`** — WordPress REST API 操作

加载方式：

```
# .agents 技能用 read_file 直接读取
read_file(path="/root/.agents/skills/sakurairo-theme/SKILL.md")
read_file(path="/root/.agents/skills/sakurairo-arcaea-blog-skill/SKILL.md")

# Hermes 内置技能用 skill_view
skill_view(name="wordpress")
```

---

## CSS 设计体系

### 核心审美

Arcaea 的视觉气质是 **"遥远、冰冷、孤独"**——白色空间 + 碎裂玻璃、终末感天空 + 数字废墟、科幻感 UI + 电子音乐、抽象叙事 + 情绪片段。

博客风格 = 数字遗迹考古感。阅读是在废墟中拾取碎片。

其本质是一种 **"未来感 × 情绪化 × 极简秩序 × 崩坏诗意"**（Cyber Minimalism + Emotional Futurism）。

它与普通赛博朋克的区别：Arcaea 不追求"霓虹城市"，而是 **空旷、孤独、神圣感、几何秩序、数据碎片、光污染极少、高透明度、低饱和、情绪压抑与希望并存**。

### 配色体系

```css
/* 主色 — 冰蓝/冷白/淡紫 */
--c-primary: #dbe8ff;
--c-accent: #9db4ff;
--c-skyblue: #8ad8ff;
--c-violet: #c7b6ff;
--c-white: #ffffff;

/* 背景 — 深色太空 */
--bg-deep: #05070b;
--bg-mid: #0b1020;
--bg-surface: #111827;
--bg-dark: #09090f;
```

**禁止**：高纯度红、荧光绿、彩虹渐变、过亮蓝。

### 视觉层级体系

正确结构 = **"很多层轻重不同的小玻璃"**，而非一块巨型玻璃面板。

```
背景图（最底层）
  ↓
bg-overlay（压暗背景，fixed 定位）
  ↓
容器/分类（轻，透明度 ~0.42，仅分区）
  ↓
条目卡片（深，透明度 0.82，真正阅读区）
  ↓
强调块/引用（最实体，渐变底 + 左边框）
```

**核心规则**：每层视觉重量递增，透明度递减。

<<<<<<< HEAD
### 最终校准 CSS 值（v5.4 — Games + Music 统一）
=======
### 最终校准 CSS 值（v5.3 — 与线上 Games 页面一致）
>>>>>>> 8fcf6a2 (v1.6.0: 全站文章统一 Arcaea 样式 + 英文文章翻译)

```css
/* ===== 动态光晕（来自 Music 页） ===== */
.bg-glow-1 {
  position: fixed;
  width: 1000px; height: 1000px;
  top: -200px; right: -150px;
  background: radial-gradient(circle, rgba(120,180,255,0.08), transparent 70%);
  filter: blur(120px);
  pointer-events: none;
  z-index: 0;
  animation: floatGlow 20s ease-in-out infinite;
}
.bg-glow-2 {
  position: fixed;
  width: 800px; height: 800px;
  bottom: -150px; left: -100px;
  background: radial-gradient(circle, rgba(167,139,250,0.06), transparent 70%);
  filter: blur(100px);
  pointer-events: none;
  z-index: 0;
  animation: floatGlow 24s ease-in-out infinite reverse;
}
@keyframes floatGlow {
  0%   { transform: translate(0,0) scale(1); }
  50%  { transform: translate(50px,-30px) scale(1.06); }
  100% { transform: translate(0,0) scale(1); }
}

/* ===== 噪声纹理层（来自 Music 页） ===== */
.games-arcaea-wrap::before {
  content: "";
  position: fixed;
  inset: 0;
  background-image: url("data:image/svg+xml,%3Csvg viewBox='0 0 256 256' xmlns='http://www.w3.org/2000/svg'%3E%3Cfilter id='n'%3E%3CfeTurbulence type='fractalNoise' baseFrequency='0.85' numOctaves='4' stitchTiles='stitch'/%3E%3C/filter%3E%3Crect width='100%25' height='100%25' filter='url(%23n)' opacity='0.04'/%3E%3C/svg%3E");
  opacity: 0.4;
  pointer-events: none;
  z-index: -2;
}

/* ===== 背景遮罩层 ===== */
.games-arcaea-wrap .bg-overlay {
  position: fixed;
  inset: 0;
  background: rgba(0, 0, 0, 0.62);
  backdrop-filter: blur(20px) brightness(0.68) saturate(60%);
  -webkit-backdrop-filter: blur(20px) brightness(0.68) saturate(60%);
  z-index: 0;
  pointer-events: none;
}

/* ===== 全局覆盖 ===== */
:root {
  --theme-skin: #0a0e18;
  --theme-skin-dark: #05070d;
  --global-font-weight: 300;
}

/* ===== 动态光晕 ===== */
.bg-glow-1{position:fixed;width:1000px;height:1000px;top:-200px;right:-150px;background:radial-gradient(circle,rgba(120,180,255,0.08),transparent 70%);filter:blur(120px);pointer-events:none;z-index:0;animation:floatGlow 20s ease-in-out infinite}
.bg-glow-2{position:fixed;width:800px;height:800px;bottom:-150px;left:-100px;background:radial-gradient(circle,rgba(167,139,250,0.06),transparent 70%);filter:blur(100px);pointer-events:none;z-index:0;animation:floatGlow 24s ease-in-out infinite reverse}

/* ===== 内容容器 ===== */
.games-arcaea-wrap {
  position: relative;
  z-index: 1;
  max-width: 100%;
  color: #f7fbff;
  overflow-x: hidden;
}

/* ===== HERO（v5.4: 禁用 sticky 防止内容挤占）===== */
.games-arcaea-wrap .game-hero{padding:120px 0 80px;display:flex;align-items:flex-start;justify-content:space-between;min-height:70vh;max-width:1380px;margin:0 auto;padding-left:clamp(24px,6vw,90px);padding-right:clamp(24px,6vw,90px);gap:clamp(24px,5vw,60px)}
.hero-left{flex:1 1 auto;max-width:620px;min-width:0}
.hero-title{font-size:clamp(48px,8vw,100px);font-weight:700;letter-spacing:-0.04em;line-height:1.05;background:linear-gradient(90deg,#9ecfff,#8ba7ff,#a78bfa);-webkit-background-clip:text;-webkit-text-fill-color:transparent;background-clip:text;margin:0}
.hero-right{flex:0 0 auto;min-width:180px;text-align:right;padding-top:60px}
.floating-tag{display:block;font-size:clamp(13px,1.4vw,20px);font-weight:300;color:rgba(255,255,255,0.18);letter-spacing:0.08em;line-height:2.3;transition:color 0.4s ease;cursor:default}

/* ===== 分类容器（轻）===== */
.game-category {
  background: rgba(10, 14, 24, 0.42);
  border: 1px solid rgba(255, 255, 255, 0.06);
  border-radius: 18px;
  padding: 16px 20px;
  margin: 32px 0;
  backdrop-filter: blur(10px);
  -webkit-backdrop-filter: blur(10px);
  box-shadow: none;
}

/* ===== 条目卡片（重）===== */
.game-entry {
  background: rgba(8, 12, 20, 0.82);
  border-radius: 18px;
  padding: 20px 24px;
  margin: 16px 0;
  border: 1px solid rgba(160, 220, 255, 0.16);
  box-shadow:
    0 8px 32px rgba(0,0,0,0.32),
    inset 0 0 0 1px rgba(255,255,255,0.06);
  transition: border-color 0.3s ease, box-shadow 0.3s ease;
  backdrop-filter: blur(8px);
  -webkit-backdrop-filter: blur(8px);
}
.game-entry:hover {
  border-color: rgba(160,220,255,0.30);
  box-shadow: 0 6px 28px rgba(0,0,0,0.40);
}

/* ===== 色调变体（按分类，类似 Music 页 artist-card--{artist}） ===== */
.game-entry--core {
  background: linear-gradient(145deg, rgba(139,167,255,0.10), rgba(219,234,254,0.04));
  border-color: rgba(139,167,255,0.18);
}
.game-entry--rhythm {
  background: linear-gradient(145deg, rgba(177,140,255,0.10), rgba(199,160,255,0.04));
  border-color: rgba(177,140,255,0.18);
}
.game-entry--narrative {
  background: linear-gradient(145deg, rgba(100,140,255,0.08), rgba(80,120,230,0.03));
  border-color: rgba(100,140,255,0.16);
}
.game-entry--exploration {
  background: linear-gradient(145deg, rgba(120,210,255,0.08), rgba(160,230,255,0.03));
  border-color: rgba(120,210,255,0.16);
}
.game-entry--engineering {
  background: linear-gradient(145deg, rgba(150,160,180,0.07), rgba(130,140,160,0.03));
  border-color: rgba(150,160,180,0.14);
}
.game-entry--flight {
  background: linear-gradient(145deg, rgba(255,160,80,0.07), rgba(255,180,100,0.03));
  border-color: rgba(255,160,80,0.14);
}
.game-entry--darkfantasy {
  background: linear-gradient(145deg, rgba(255,80,100,0.07), rgba(200,60,80,0.03));
  border-color: rgba(255,80,100,0.14);
}

/* ===== 分类标题 ===== */
.game-category > h2 {
  color: #f3f0ff;
  font-size: 1.75em;
  font-weight: 700;
  letter-spacing: 0.04em;
  margin: 0 0 8px 0;
  text-shadow:
    0 0 10px rgba(180,150,255,0.55),
    0 2px 4px rgba(0,0,0,0.75);
}

/* ===== 卡片标题 ===== */
.game-entry h3 {
  display: flex;
  align-items: center;
  gap: 10px;
  color: #e2f4ff;
  font-size: 1.35em;
  font-weight: 700;
  margin: 0 0 6px 0;
  text-shadow:
    0 0 12px rgba(126,200,255,0.45),
    0 2px 4px rgba(0,0,0,0.75);
}
.game-entry h3::before {
  content: "#";
  color: rgba(255,145,145,0.75);
  font-size: 0.95em;
  font-weight: 700;
  flex-shrink: 0;
}
```

<<<<<<< HEAD
### Section 通用布局

```
.section-kicker — 细微分类标签 (uppercase, violet-tinted)
.section-title  — H2 标题 (text-shadow 紫辉)
.section-desc   — 段落描述 (淡蓝灰)
```

### Blockquote 设计（Sakurairo 覆盖 — 必须 !important）
=======
**v5.3 新增（vs v5.2）**：
- 动态光晕 `bg-glow-1/2` + `@keyframes floatGlow`（从 Music 页移植）
- 噪声纹理层 `::before`（fractalNoise SVG data URI）
- 卡片色调变体 `game-entry--{category}`（7 种色调，类比 Music 页 artist-card--{artist}）
- Hero 区域：`game-hero` + `hero-left/right` + `floating-tag`
- Section 三段式：`section-kicker` + `section-title` + `section-desc`
- Mood spectrum：`mood-spectrum` + `mood-tag`（4 种 hover 变体）
- `prefers-reduced-motion` fallback
- bg-overlay blur 从 18px → 20px，alpha 从 0.58 → 0.62
- `color-scheme: dark` 适配

### 文章包裹 CSS（Article Wrapper — v1.6.0 新增）

全站 43 篇文章统一 Arcaea 风格的精简 CSS 模板。与 Games/Music 页面的完整页面风格不同，文章包裹 CSS 专注于**阅读体验**：
- 代码块毛玻璃（FiraCode + blur(18px)）
- Blockquote 渐变底 + 冰蓝左边框（覆盖 Sakurairo FontAwesome）
- h2 底部细线分隔，h3 `#::before` 标记
- 表格暗玻璃卡片样式
- 行内 code 半透明蓝底
- `prefers-reduced-motion` 暗模式适配

**完整 CSS 模板文件**：`references/article-wrapper-css.md`

**压缩版（直接用于文章）**：
```css
:root{--arcaea-bg:rgba(14,18,28,0.72);--arcaea-border:rgba(160,220,255,0.12);--arcaea-primary:#b0dcff;--arcaea-accent:#9db4ff;--arcaea-text:#f0f6ff;--arcaea-muted:#a0b8d8;--arcaea-hash:rgba(255,130,130,0.55)}.arcaea-article-content{position:relative;z-index:1;color:var(--arcaea-text);max-width:100%}.arcaea-article-content h2{color:#e8f0ff;font-size:1.65em;font-weight:700;margin-top:2em;margin-bottom:0.6em;padding-bottom:0.3em;border-bottom:1px solid rgba(160,220,255,0.15);text-shadow:0 0 10px rgba(180,150,255,0.30),0 2px 4px rgba(0,0,0,0.50)}.arcaea-article-content h3{display:flex;align-items:center;gap:10px;color:#e2f4ff;font-size:1.35em;font-weight:700;margin-top:1.5em;margin-bottom:0.5em;text-shadow:0 0 12px rgba(126,200,255,0.25),0 2px 4px rgba(0,0,0,0.50)}.arcaea-article-content h3::before{content:"#";color:var(--arcaea-hash);font-size:0.9em;font-weight:700;flex-shrink:0}.arcaea-article-content p{line-height:1.8;margin:1em 0;color:#f0f6ff}.arcaea-article-content pre{font-family:"FiraCode Nerd Font","Fira Code",Consolas,monospace!important;font-size:15px;line-height:1.7;background:rgba(15,18,22,0.72)!important;border:1px solid rgba(160,220,255,0.18);border-radius:16px;backdrop-filter:blur(18px);box-shadow:0 8px 24px rgba(0,0,0,0.28),0 0 0 1px rgba(255,255,255,0.02) inset;padding:1.35rem 1.5rem;margin:2rem 0;overflow:auto}.arcaea-article-content code{font-family:"FiraCode Nerd Font","Fira Code",Consolas,monospace!important;background:rgba(160,220,255,0.08);padding:0.2em 0.4em;border-radius:4px;font-size:0.9em}.arcaea-article-content pre code{background:transparent;padding:0;border-radius:0;font-size:inherit}.arcaea-article-content blockquote{background:linear-gradient(135deg,rgba(40,70,120,0.22),rgba(20,30,50,0.14));border-left:3px solid rgba(126,200,255,0.55);border-radius:12px;padding:14px 20px!important;margin:14px 0!important;color:#f5faff!important;box-shadow:none!important}.arcaea-article-content blockquote::before{display:none!important;content:none!important}.arcaea-article-content blockquote::after{display:none!important;content:none!important}.arcaea-article-content table{border-collapse:collapse;width:100%;margin:1.5em 0;background:rgba(10,14,24,0.42);border-radius:12px;overflow:hidden}.arcaea-article-content th,.arcaea-article-content td{padding:10px 14px;border:1px solid rgba(255,255,255,0.06);text-align:left}.arcaea-article-content th{background:rgba(139,167,255,0.10);color:#c8ddff;font-weight:600}.arcaea-article-content ul,.arcaea-article-content ol{padding-left:1.5em;margin:0.8em 0}.arcaea-article-content li{margin:0.4em 0;line-height:1.7}.arcaea-article-content a{color:#8ad8ff;text-decoration:none;border-bottom:1px solid rgba(138,216,255,0.25);transition:border-color 0.2s}.arcaea-article-content a:hover{border-bottom-color:rgba(138,216,255,0.6)}.arcaea-article-content hr{border:none;height:1px;background:linear-gradient(90deg,transparent,rgba(160,220,255,0.2),transparent);margin:2em 0}.arcaea-article-content img{border-radius:12px;max-width:100%;height:auto}.arcaea-article-content figure{margin:1.5em 0}.arcaea-article-content figcaption{text-align:center;font-size:0.85em;color:var(--arcaea-muted);margin-top:0.5em}.arcaea-article-content .wp-block-heading{color:inherit}.arcaea-article-content .wp-block-paragraph{color:inherit}.arcaea-article-content .wp-block-table{overflow-x:auto}@media(prefers-reduced-motion:reduce){.arcaea-article-content *{animation:none!important;transition:none!important}}
```

### 配色 Token
>>>>>>> 8fcf6a2 (v1.6.0: 全站文章统一 Arcaea 样式 + 英文文章翻译)

```css
.games-arcaea-wrap blockquote {
  background: linear-gradient(135deg, rgba(40,70,120,0.22), rgba(20,30,50,0.14)) !important;
  border-left: 3px solid rgba(126,200,255,0.55) !important;
  border-radius: 12px !important;
  padding: 14px 20px !important;
  margin: 14px 0 !important;
  color: #f8fbff !important;
  font-size: 1.06em !important;
  line-height: 1.85 !important;
  box-shadow: none !important;
  border-right: none !important;
  border-top: none !important;
  border-bottom: none !important;
}
.games-arcaea-wrap blockquote::before,
.games-arcaea-wrap blockquote::after {
  display: none !important;
  content: none !important;
}
.games-arcaea-wrap blockquote p {
  margin: 0 !important;
  padding: 0 !important;
  padding-left: 0 !important;
  border: none !important;
  background: none !important;
}
```

### 标题 `#` 标记

绝对禁止在 HTML 中写独立 `#` 文本节点。正确做法：

```css
.game-entry h3 { display: flex; align-items: center; gap: 10px; }
.game-entry h3::before { content: "#"; color: rgba(255,145,145,0.75); font-size: 0.95em; font-weight: 700; flex-shrink: 0; }
```

### Music 页面专属组件

详见 [`references/mood-spectrum-css.md`](references/mood-spectrum-css.md) 和下方 CSS。

```css
/* Artist Grid */
.artist-grid{display:grid;grid-template-columns:repeat(3,minmax(0,1fr));gap:22px;margin:24px 0}
.artist-card{...background:rgba(8,12,20,0.82);border:1px solid rgba(160,220,255,0.16);...}
.artist-card h3::before{content:"#";color:rgba(255,145,145,0.75)}

/* Album Wall */
.album-wall{display:grid;grid-template-columns:repeat(auto-fill,minmax(132px,1fr));gap:15px}

/* Resonance */
.resonance-line{padding-left:24px;border-left:none}
.resonance-line::before{content:">";position:absolute;left:0;color:rgba(157,180,255,0.62)}
.resonance-quote{margin:26px 0;padding:16px 0 16px 24px;border-left:3px solid rgba(126,200,255,0.48);border-radius:0 12px 12px 0}
```

### Mood Spectrum（情绪标签）

关键见 [`references/mood-spectrum-css.md`](references/mood-spectrum-css.md)。核心要点：
- `flex-shrink: 0` 防止标签被压缩
- `display: inline-flex` 使内容垂直居中
- `gap: 10px 12px`（行距10px + 列距12px）
- **修补前检查是否已有前缀**，避免 `.games-arcaea-wrap .games-arcaea-wrap` 双重污染

### Arcaea Lite（Hub 页面 / 博客文章轻量包裹）

<<<<<<< HEAD
见 [`references/arcaea-lite-wrapper.md`](references/arcaea-lite-wrapper.md)。用于关于、工具箱、嵌入式专题、游记、**博客文章**等非内容密集型页面。带 `bg-glow` 光晕氛围但**无 bg-overlay 模糊层**（避免覆盖正文可读性）。
=======
    border: 1px solid rgba(160,220,255,.18);

    border-radius: 16px;

    backdrop-filter: blur(18px);

    box-shadow:
        0 8px 24px rgba(0,0,0,.28),
        0 0 0 1px rgba(255,255,255,.02) inset;

    padding: 1.35rem 1.5rem;

    margin: 2rem 0;

    overflow: auto;
}

/* copy button */
div.code-toolbar > .toolbar button {
    background: rgba(255,255,255,.08) !important;
    border: 1px solid rgba(255,255,255,.08) !important;
    color: #d8f3ff !important;
    border-radius: 8px !important;
    backdrop-filter: blur(10px);
}

/* 去掉 token 灰块 */
.token.operator,
.token.entity,
.token.url {
    background: transparent !important;
}
```

### LightGallery 配置

```json
{
  "plugins": [
    "lgZoom",
    "lgHash"
  ],
  "speed": 500,
  "download": false,
  "counter": false,
  "hideBarsDelay": 2000,
  "backdropDuration": 300,
  "selector": ".entry-content img"
}
```

### Mood Spectrum（情绪标签云）

```css
.mood-spectrum {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
  padding: 8px 0;
}
.mood-tag {
  padding: 6px 14px;
  border-radius: 999px;
  background: rgba(255,255,255,0.04);
  border: 1px solid rgba(255,255,255,0.08);
  color: rgba(255,255,255,0.78);
  font-size: 0.85em;
  transition: all 0.3s ease;
  cursor: default;
  white-space: nowrap;
}
.mood-tag:hover {
  background: rgba(139,167,255,0.10);
  border-color: rgba(139,167,255,0.22);
  transform: translateY(-2px);
  box-shadow: 0 0 24px rgba(139,167,255,0.08);
}
.mood-tag--red:hover { background: rgba(255,107,129,0.10); border-color: rgba(255,107,129,0.22); }
.mood-tag--violet:hover { background: rgba(177,140,255,0.10); border-color: rgba(177,140,255,0.22); }
.mood-tag--gray:hover { background: rgba(209,213,219,0.08); border-color: rgba(209,213,219,0.18); }
```

**wpautop 坑**：所有 `<span>` 必须写在同一行，否则标签间会被插入 `<br />` 导致 flex-wrap 失效。

### Album Wall（曲绘墙/卡片网格）

```css
.album-wall {
  display: grid;
  grid-template-columns: repeat(auto-fill, minmax(110px, 1fr));
  gap: 12px;
}
.album-card {
  border-radius: 12px;
  background: rgba(255,255,255,0.04);
  backdrop-filter: blur(14px) saturate(80%);
  border: 1px solid rgba(255,255,255,0.06);
  aspect-ratio: 1;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  padding: 10px;
  text-align: center;
  transition: transform 0.35s ease, box-shadow 0.35s ease, border 0.35s ease;
}
.album-card:hover {
  transform: translateY(-4px) scale(1.03);
  border-color: rgba(139,167,255,0.20);
  box-shadow: 0 0 30px rgba(139,167,255,0.10);
}
.album-card .album-art {
  width: 100%; aspect-ratio: 1;
  border-radius: 10px;
  margin-bottom: 8px;
  filter: brightness(0.85) contrast(1.05);
  transition: transform 0.45s ease, filter 0.45s ease;
  background-size: cover; background-position: center;
}
.album-card:hover .album-art { transform: scale(1.06); filter: brightness(1) contrast(1.08); }
.album-title { font-size: 0.78em; font-weight: 600; color: #edf4ff; margin: 0; }
.album-artist { font-size: 0.68em; color: rgba(255,255,255,0.45); margin: 1px 0 0; }
```

**尺寸说明**：`minmax(110px, 1fr)` 在 1380px 容器中产生 8-10 列，每列 ~110-130px，适合 8 张专辑封面展示。大于此值（如 200px）会减少到 5 列，每张卡片过大。

### 设计原则速查

| 原则 | 含义 |
|------|------|
| 三层分离 | 背景图 → bg-overlay(blur+brightness+saturate) → 内容容器 |
| 轻重分配 | 容器轻(~0.42, box-shadow:none) + 卡片重(~0.82) + 玻璃heavy(~0.88) |
| text-shadow 标题 | 分类标题 `0 0 10px` 紫辉，卡片标题 `0 0 12px` 蓝辉 |
| 容器无阴影 | `box-shadow: none` 是关键 |
| 卡片去独立 blur | 复用父容器 blur，避免 stutter |
| 单引号 | 仅保留左上或无引号，`::after` 禁 |
| # 绑定标题 | `h3::before`，绝对不独立文本节点 |
| ice blue | `rgba(160,220,255)` 主色，不用荧光 cyan |
| !important 覆盖 | blockquote 的 padding/伪元素/子元素 |
| 代码块玻璃化 | `pre[class*="language-"]` 毛玻璃卡片风格，15px，blur(18px) |
| token 去灰底 | `.token.operator/.entity/.url` 设 `background: transparent` |
| LightGallery | `selector: ".entry-content img"`，关 download/counter |
| 柔光非霓虹 | 模糊辉光 `box-shadow: 0 0 20px rgba(120,180,255,.15)`，非霓虹/RGB |
| 浮空感 | 元素必须有漂浮轻质感，非 Bootstrap 厚重卡片 |
| 纵向空间 | 大量 padding-top/bottom，页面不拥挤 |
| 信息层级少 | 不堆内容/按钮/文字，少但精 |

### Arcaea Web Design AI Prompt（可直接使用）

```text
Design a futuristic emotional web UI inspired by Arcaea.

Style:
- cyber minimalism
- atmospheric sci-fi
- floating holographic panels
- glassmorphism
- soft ambient glow
- dark abstract space
- music visualization aesthetics
- emotional and lonely atmosphere
- elegant geometric composition
- subtle gradients
- transparent layered interface
- low saturation
- clean typography
- sacred futuristic feeling

Avoid:
- colorful cyberpunk
- RGB gaming style
- overcrowded layout
- cartoonish UI
- neon overload
- thick borders
- flat corporate design

Use:
- blur layers
- floating cards
- soft white/blue glow
- arc-shaped decorative lines
- abstract particles
- smooth breathing animation
- cinematic spacing
- minimalist navigation
- rhythm-game inspired composition

Mood:
lonely, ethereal, futuristic, emotional, immersive
```

### 推荐配套技术栈

| 技术 | 用途 |
|------|------|
| GSAP | 漂浮动画、时间线控制 |
| Lenis | 丝滑惯性滚动 |
| Framer Motion | React 动画 |
| Three.js | 空间粒子、3D 背景 |
| Vanta.js | 星空/粒子背景 (基于 Three.js) |
| Shaders (GLSL) | Arc 光效、轨迹辉光 |
| Prism.js | 技术感代码高亮（玻璃卡片风格） |
| Mermaid | 数据流程图、状态图 |
>>>>>>> 8fcf6a2 (v1.6.0: 全站文章统一 Arcaea 样式 + 英文文章翻译)

---

## Sakurairo 主题集成

### CSS 变量覆盖

```css
:root {
  --theme-skin: #0a0e18;              /* 覆盖主题主色为深色底 */
  --theme-skin-dark: #05070d;          /* 暗色模式更深 */
  --global-font-weight: 300;           /* 细体，符合 Arcaea 空灵感 */
}
```

**已知问题**：SKILL.md 记录的全局 `--theme-skin` 覆盖值（`#0a0e18`）在线上仍未部署。当前实际值为 `#505050`（灰色）。部署方法：在 Sakurairo 主题选项 → 全局设置 → 自定义样式中添加上述 CSS。检查方法：查看首页 HTML 中 `--theme-skin` 的值。

### 自定义 CSS 注入路径

Sakurairo 支持两处自定义 CSS 注入：
- **主题选项 → 全局设置 → 自定义样式** — 注入 `<head>`，持久稳定（放 Prism/字体等全局样式）
- **页面/文章中的 `<style>` 块** — 作用域仅限于该页面（放 Arcaea 玻璃卡片等页面独有样式）

### 已知冲突（Sakurairo v3.0.10 默认）

```css
/* 主题默认（必须用 !important 覆盖） */
blockquote { padding: 20px 30px !important; }
blockquote:before { content: "\f10d" !important; }  /* FontAwesome 图标 */
blockquote:after  { content: "\f10e" !important; }
blockquote p { padding-left: 10px; }
```

### 主题 REST API 端点

| 端点 | 用途 |
|------|------|
| `GET /wp-json/sakura/v1/chatgpt?post_id={ID}` | 生成 AI 摘要 |
| `GET /wp-json/sakura/v1/archive_info` | 归档统计信息 |
| `GET /wp-json/sakura/v1/qqinfo` | QQ 用户信息 |

---

## WordPress 发布工作流

### 基础设施

- **站点**: https://babel36acl.xyz
- **主题**: Sakurairo v3.0.10
- **WP REST API**: `https://babel36acl.xyz/wp-json/wp/v2/pages/{id}` — 使用 Application Password 认证
- **MCP Adapter**: 已配置为 Hermes 原生 MCP 服务器（`hermes mcp list` 可见）。`/reload-mcp` 刷新。当前仅有 WPForms 能力。
- **凭证**: 通过 Python `urllib` + Application Password 使用，**绝不**写入文件或 `curl -u`。

### 页面更新流程

1. `GET /wp-json/wp/v2/pages/{id}?context=edit` 获取 `content.raw`（原始 HTML，未被 wpautop 污染）
2. 在 Python 中修改 `raw` 内容（精确字符串匹配）
3. `POST /wp-json/wp/v2/pages/{id}` 传回 `{"content": modified_raw}`
4. 用 `curl -sL URL | grep` 验证关键 CSS 选择器存在

**注意：MCP 工具没有 delete 操作。** 需要删除文章时，通过 Python REST API 的 `method="DELETE"` 移入回收站：

```python
req = urllib.request.Request(f"{SITE}/wp-json/wp/v2/posts/{post_id}",
    method="DELETE", headers=HEADERS)
```

### 已知页面 ID

| ID | 名称 | 风格 | 备注 |
|----|------|------|------|
| 375 | 🎮 游戏 | Arcaea v5.4 完整 | game-entry 卡片系统 |
| 430 | 🌌 音乐 | Arcaea v5.4 完整 | 已统一 wrapper 类名 |
| 190 | 关于 | Arcaea Lite | `.arcaea-wrap` 轻量包裹 |
| 188 | 工具箱 | Arcaea Lite | 同上 |
| 182 | 嵌入式专题 | Arcaea Lite | 同上 |
| 373 | 游记 | Arcaea Lite | 同上 |
| 186 | 架构与重构 | 需主题 CSS | **博客列表页**，模板覆盖内容 |
| 184 | 工程复盘 | 需主题 CSS | **博客列表页**，模板覆盖内容 |

### 已知分类

| ID | 名称 | slug |
|----|------|------|
| 1  | 未分类 | uncategorized |
| 6  | 学习笔记 | %e5%ad%a6%e4%b9%a0%e7%ac%94%e8%ae%b0 |
| 13 | 嵌入式实战 | embedded-practice |
| 14 | 工程复盘 | engineering-retrospective |
| 15 | 架构与重构 | architecture-and-refactoring |
| 16 | 方法与工具 | methods-and-tools |

### 已知标签

| ID | 名称 | slug |
|----|------|------|
| 11 | ARCH | arch |
| 8  | HPM | hpm |
| 9  | RISC-V | risc-v |
| 10 | WSL2 | wsl2 |

### 规则：Python REST API 优先，绝对禁止 curl 带密码

**永远不要**在命令行使用 `curl -u "user:pass"` 形式访问 WordPress REST API。凭证会被 shell history 和进程列表捕获。

正确做法：

1. **Python + urllib + Application Password**（当前唯一可行路径）— 凭证不落磁盘，不暴露于 shell history。使用 `execute_code` 运行。完整脚本模式见 [`references/publishing-python-pattern.md`](references/publishing-python-pattern.md)。
2. **手动粘贴** — 将生成的 HTML 提供给用户，通过 WordPress 后台 Text 模式粘贴（安全且可靠的回退方案）。
3. **MCP 工具** — 当前仅有 WPForms 能力，无法直接发布/更新文章和页面。

MCP Adapter 配置查看：`hermes mcp list`，重载：`/reload-mcp`。

### 发布 Posts（新文章）流程

1. 列出已有文章避免重复：`GET /wp-json/wp/v2/posts?per_page=100&_fields=id,title,slug`
2. 列出分类和标签，确认分类 ID：`GET /wp-json/wp/v2/categories`、`GET /wp-json/wp/v2/tags`
3. 构建文章的 HTML 内容（含 `<style>` Arcaea Lite 包裹层）
4. `POST /wp-json/wp/v2/posts` 传参 `{title, slug, content, categories[], tags[], status="publish", excerpt}`
5. 额外加 `excerpt` 字段用于文章摘要

### 发布 Pages（更新已有页面）流程

1. `GET /wp-json/wp/v2/pages/{id}?context=edit` 获取 `content.raw`（未被 wpautop 污染的原始 HTML）
2. 在 Python 中修改 `raw` 内容（精确字符串匹配或替换）
3. `POST /wp-json/wp/v2/pages/{id}` 传回 `{"content": modified_raw}`
4. 用 `curl -sL URL | grep` 验证关键 CSS 选择器存在

### 发布后的验证

发布后必须验证：
1. REST API 返回 `status: publish`
2. 页面 HTTP 200 可访问
3. CSS style 块未被 wpautop 破坏（检查页面源码中 `<style>` 标签完整性，用 Python `urllib` 或 `curl` 实现，均**不携带密码**）
4. `<pre>` 代码块在渲染后的页面中可见（通过 grep 确认）
5. `arcaea-wrap` 类名存在于渲染后的 HTML 中（确认 Arcaea 样式正确加载）

---

## 工作流

### 0. 内容审查（写新内容前必须先做）

**永远不要跳过这一步。** 列出已有文章以发现空白，避免重复。

```python
# 列出所有 posts 和 pages
req = urllib.request.Request(f"{site}/wp-json/wp/v2/posts?per_page=100&_fields=id,title,slug", headers=headers)
req2 = urllib.request.Request(f"{site}/wp-json/wp/v2/pages?per_page=50&_fields=id,title,slug", headers=headers)
```

检查输出，确认没有已存在相似内容的文章/页面后再动笔。

### 1. 写新博客文章

a. 完成步骤 0 的空缺分析
b. 加载前置技能和 Arcaea Lite 包裹层 CSS
c. 撰写文章 HTML：`<style>Arcaea Lite CSS</style>` + `<div class="arcaea-wrap"><div class="bg-glow-1">...</div><div class="bg-glow-2">...</div>`
d. 使用 Python + urllib + Application Password 发布到 `POST /wp-json/wp/v2/posts`
   - 必须传参: `title`, `slug`, `content`, `categories[]`, `status="publish"`
   - 可选: `excerpt`, `tags[]`
   - slug 用英文 kebab-case
   - excerpt 写 50-120 字的中文摘要
e. 验证: HTTP 200 + 文章可访问 + `<style>` 未被 wpautop 破坏 + `<pre>` 代码块可见 + `arcaea-wrap` 类名存在

### 2. 设计新页面

a. 加载前置技能
b. 使用三层背景结构 + Hero + 卡片网格模式作为起始骨架
c. 使用 v5.4 精确 CSS 数值应用 Arcaea 层级
d. 添加 Sakurairo 主题 `!important` 覆盖
e. 通过 Python REST API 或手动粘贴发布

### 3. 美化已有页面 → 应用 Arcaea 风格

a. 分析当前页面 CSS 结构
b. 统一 wrapper 类名为 `.games-arcaea-wrap`
c. 统一 overlay 类名为 `.bg-overlay`
d. 按优先级修复：
   - **P0**: 统一类名体系（`.page-music` → `.games-arcaea-wrap`）
   - **P1**: `#` 独立文本 → `h3::before`
   - **P2**: 删除右下引号（blockquote ::before/::after → display:none）
   - **P3**: 卡片背景压暗 + 标题 text-shadow
   - **P4**: 正文加亮
   - **P5**: hero-right 禁用 sticky（防止内容挤占）

<<<<<<< HEAD
### 4. 修复 CSS 冲突
=======
### 2b. 升级已有 Arcaea 页面 → 同步 Music 页丰富度

当目标页面（如 Games）需要与 Music 页（Page 430）的视觉丰富度对齐时：

a. 添加动态光晕 (`bg-glow-1`/`bg-glow-2` + `@keyframes floatGlow`)
b. 添加噪声纹理层 (`::before` + fractalNoise SVG data URI)
c. 升级页面 Intro 为 Hero 区域（渐变标题 + 右侧浮动标签 floating-tag）
d. 为每个区块添加 section-kicker / section-title / section-desc 三段结构
e. 为卡片添加按分类的色调变体（如 game-entry--core/rhythm/narrative 等），参考 Music 页的 artist-card--onoken/akq/fery 模式
f. 添加 mood-spectrum 情绪标签云
g. 校验：添加 `@media (prefers-reduced-motion: reduce)` 禁用动画
h. 校验：用 `web-design-guidelines` 技能审查（focus-visible, compositor-friendly transition, no `transition: all`）

完整对照表：

| Music 页模式 | Games 页对应 | CSS 实现 |
|-------------|-------------|---------|
| `bg-glow-1/2 + floatGlow` | `bg-glow-1/2 + floatGlow` | `radial-gradient + blur(120px) + animation` |
| `page-music::before` (noise) | `games-arcaea-wrap::before` | `fractalNoise SVG data URI` |
| `music-hero + hero-left/right` | `game-hero + hero-left/right` | `flex + sticky hero-right + floating-tag` |
| `artist-card--{artist}` | `game-entry--{category}` | `linear-gradient(145deg, ...) + border-color` |
| `section-kicker/title/desc` | `section-kicker/title/desc` | `.85em uppercase + clamp(28px,3.5vw,48px) + 16px/300` |
| `mood-spectrum + mood-tag` | `mood-spectrum + mood-tag` | `flex-wrap: wrap + border-radius: 999px + hover:translateY(-2px)` |

### 3. 修复 CSS 冲突
>>>>>>> 8fcf6a2 (v1.6.0: 全站文章统一 Arcaea 样式 + 英文文章翻译)

a. 对冲突选择器用 `!important` 覆盖
b. `blockquote::before` 和 `blockquote::after` 需同时设置 `display: none !important` 和 `content: none !important`
c. wpautop 会在 CSS 中插入 `<p>` 标签——使用紧凑单行 CSS 避免空行

### 5. 全站文章批量统一样式（v1.6.0 新增）

需求：「所有文章统一 Arcaea 风格」。

a. 准备统一 CSS 模板（见 `references/article-wrapper-css.md` 的压缩版）
b. 遍历所有文章，对每篇内容执行：
   - 移除已存在的 `<style>` 块（替换为统一模板）
   - 移除嵌入的完整 HTML 文档（`<!DOCTYPE>`、`<html>`、`<head>`、`<body>`）
   - 移除 hash-in-text（`<p># ` 等——改用 h3::before）
   - 添加 `<style>` 块 + `<div class="arcaea-article-content">` 包裹
c. 通过 WordPress REST API 批量更新
d. 验证：随机抽检 3-5 篇文章，确认样式块、包裹层、blockquote 覆盖完整

**批量更新脚本**见 `references/article-wrapper-css.md` 全站批量应用脚本部分。

**特殊处理**：
- 如果文章有英文正文 → 翻译为中文后再包裹
- 如果文章有杂乱内联样式（如 ID 766 嵌入的完整 HTML）→ 剥离后再包裹
- CSS 被 wpautop 插入 `<p>` 不影响浏览器渲染，可忽略

---

## Pitfalls

### 1. wpautop 污染 `<style>` 标签
WordPress 的 wpautop 会在 `<style>` 块内的每个换行处插入 `<p>` 标签。**解决方案**：CSS 规则写成紧凑单行，规则之间不留空行。虽然浏览器仍能解析，但会臃肿且丑陋。

### 2. hero-right sticky 造成内容挤占
旧版使用 `position: sticky; top: 25vh` 让右侧标签浮动跟随滚动，导致滚动时漂浮标签压入下方内容区。**v5.4 修复**：移除此属性，改回普通 flex 布局。

### 3. Music 页面类名不一致
历史版本中 Music 页面使用 `.page-music` / `.bg-overlay-music` / `.music-hero` 等独立类名，与 Games 页面的 `.games-arcaea-wrap` / `.bg-overlay` / `.game-hero` 完全不同。**v5.4 统一**：所有页面使用相同的 `.games-arcaea-wrap` 体系。

### 4. Python 直接 REST API 发布路径（MCP 不可用时的正确做法）
MCP Adapter 当前仅有 WPForms 能力，无法发帖。wp_mcp_server.py 不存在。使用 Python `urllib` + Application Password 直接调 REST API 发布/更新文章和页面，详见 [`references/publishing-python-pattern.md`](references/publishing-python-pattern.md)。**不要在发布路径上死磕 MCP。**

### 5. Sakurairo 暗黑模式覆盖
Sakurairo 暗黑模式通过 `[theme-mode="dark"]` 选择器覆盖颜色，Arcaea 深色方案可能被覆盖。需在页面 CSS 中添加 `!important` 防御。

### 6. backdrop-filter 性能
不超过 3 层 blur，否则滚动卡顿。本设计仅使用 2 层（bg-overlay + game-category），安全。

### 7. alpha < 0.04 的边框在深色背景上不可见
在 `#05070d` 基底上最小可用 alpha 为 0.06。

### 8. CSS 注释污染 meta
CSS 注释中的文字可能被 WordPress 提取到 `<meta description>`。保持注释简洁。

### 9. 外部技能加载路径
本技能引用的 `sakurairo-theme`、`glassmorphism-ui` 等技能位于 `/root/.agents/skills/`，**不在 Hermes 内置技能库中**。使用 `read_file` 直接读取，而非 `skill_view`。

### 10. 双重前缀陷阱（2026-05-28 发现）
对已有 `.games-arcaea-wrap .mood-*` 选择器再次追加前缀，会变成 `.games-arcaea-wrap .games-arcaea-wrap .mood-*`，匹配不到任何元素（页面只有一层 wrap），CSS 完全失效。**修补已有选择器前，先用 regex 检查是否已含目标前缀。**

### 11. 博客列表页的模板覆盖
Sakurairo 中某些页面（架构与重构 ID=186、工程复盘 ID=184）使用 post-loop 模板，**页面内容被完全忽略**，直接显示文章列表。这类页面的 Arcaea 样式无法通过 page content 注入，必须通过**主题选项 → 自定义样式**全局注入 CSS 覆盖。

### 12. `context=edit` 获取原始内容
更新页面时使用 `?context=edit` 获取 `content.raw`（wpautop 之前的原始内容），而非默认的 `content.rendered`（已被 wpautop 污染）。`raw` 中的 HTML 结构精确可匹配，`rendered` 中被插入了不可预测的 `<p>` 标签。

### 14. `<pre>` 标签在 WordPress REST API 中可正常保存

经过验证（2026-05-28），WordPress REST API 完整保留 `<pre>` 标签，不会被 kses 过滤或 wpautop 破坏。**不要用 Markdown 代码 fence 代替 `<pre>`** — WordPress 不做 Markdown 到 HTML 的转换。博文中的代码块必须始终使用 `<pre>` 标签，验证方法：发布后 fetch 页面源码并 grep `<pre>`。

### 15. `excerpt` 摘要应该始终填写

发布博文时始终提供 `excerpt` 字段（50-120 字），不要留空。摘要用于分类页和社交分享预览，留空时 WordPress 自动截取正文前几行，效果不可控。
使用 Python `urllib` + Application Password 直接调用 WordPress REST API 发布Posts和更新Pages。**不是回退方案，是标准方法。** 适用于 MCP 仅有 WPForms 能力、wp_mcp_server.py 不存在的环境。
```python
token = base64.b64encode(b"user:app-password").decode()
headers = {"Authorization": f"Basic {token}", "Content-Type": "application/json"}
data = json.dumps({"title":"T","content":html,"categories":[13],"status":"publish"}).encode()
req = urllib.request.Request("https://site/wp-json/wp/v2/posts", data=data, headers=headers, method="POST")
```
详见 [`references/publishing-python-pattern.md`](references/publishing-python-pattern.md)。**绝对禁止** `curl -u` 传密码（会泄露到 shell history）。

## 2026-05 月度变更摘要

### 博客发布
- 本月从 Packaging_machine_V1.0 项目提炼并发布 21 篇嵌入式工程博文 (523-547)
- 覆盖主题：三层架构、构建隔离、回调注册表、统一动作表、脉冲溢出保护、CommTask、抛物线起步、motion_seen、HMI双串口、DGUS→HMIS 协议演进、EEPROM参数持久化、看门狗双层策略、任务通知链、双工具链、BSP驱动分层、状态机引擎内部、S曲线启停、同定时器多轴联动、rcfg参数框架

### CSS 变更 (v5.5 → v5.6)
- Blockquote: padding 14px 20px → 16px 28px 16px 36px（防止引号与文字重叠）
- Blockquote: 添加 `text-indent:0!important`
- 段落: 添加 `text-indent:2em`（中文首行缩进）
- Blockquote ::before: `display:none` → `content:"(^_^)"` 颜文字装饰
- Blockquote p: 添加 `text-indent:0!important`
- 全部 21 篇已发布文章通过 WordPress REST API 批量更新

---

## 已知博客内容目录

| 类型 | 页面/文章 | ID | 风格版本 |
|------|----------|----|---------|
| 游戏展示 | Games 页面（🎮 游戏） | 375 | Arcaea v5.4 |
| 音乐展示 | Music 页面（🌌 音乐） | 430 | Arcaea v5.4（已统一） |
| 博客 | AI Agent + AGENTS.md 知识库 | 407 | 默认 + Arcaea Lite |
| 博客 | CMake Presets + ARM Toolchain | 406 | 默认 + Arcaea Lite |
| 博客 | VSCode + Cortex-Debug + OpenOCD | 405 | 默认 + Arcaea Lite |
| 博客 | WSL2 + Arch Linux 嵌入式环境 | 404 | 默认 + Arcaea Lite |
| 博客 | HPM + CMake 开发教程 | 396 | 默认 |
| 博客 | 一台 STM32 设备的 83 次修复 | 176 | 默认 |
| 博客 | 状态机退出不清理标志 | 175 | 默认 |
| 博客 | 加一层包装却不删旧入口 | 174 | 默认 |
| 博客 | 两个 Bug 互相抵消 | 173 | 默认 |
| 博客 | CubeMX GPIO 分组陷阱 | 172 | 默认 |
| 博客 | FreeRTOS configASSERT 边界 | 171 | 默认 |
| 博客 | UART DMA 静默死亡 | 170 | 默认 |
| 博客 | 三层重构靠构建系统 | 523 | Arcaea Lite ✅ |
| 博客 | 不要用超时保护开环步进 | 524 | Arcaea Lite ✅ |
| 博客 | 嵌入式10个隐藏问题 | 532 | Arcaea Lite ✅ |
| 博客 | 工业控制固件完整复盘 | 526 | Arcaea Lite ✅ 含表格 |
| 博客 | 回调注册表状态机模式 | 528 | Arcaea Lite ✅ |
| 博客 | 统一动作表执行器 | 529 | Arcaea Lite ✅ |
| 博客 | 抛物线起步+motion_seen | 530 | Arcaea Lite ✅ |
| 博客 | HMI 双串口隔离架构（已更新 HMIS） | 531 | Arcaea Lite ✅ |
| 博客 | 嵌入式10个隐藏问题 | 532 | Arcaea Lite ✅ |
| 博客 | EEPROM 参数持久化迁移 | 533 | Arcaea Lite ✅ |
| 博客 | DGUS→HMIS 协议演进 | 534 | Arcaea Lite ✅（已重写）|
| 博客 | 看门狗双层策略 | 535 | Arcaea Lite ✅ |
| 博客 | FreeRTOS 任务通知链 | 536 | Arcaea Lite ✅ |
| 博客 | 双工具链构建系统 | 537 | Arcaea Lite ✅ |
| 博客 | 软件 I2C + EEPROM BSP 驱动 | 538 | Arcaea Lite ✅ |
| 博客 | 状态机引擎内部协同 | 539 | Arcaea Lite ✅ |
| 博客 | 步进 S 曲线 done 置位时机 | 540 | Arcaea Lite ✅ |
| 博客 | BSP 实现总览 10 驱动模式 | 541 | Arcaea Lite ✅ |
| 博客 | HMIS 会话协议取代 DGUS | 542 | Arcaea Lite ✅ |
| 博客 | rcfg 参数框架内部 | 543 | Arcaea Lite ✅ |
| 博客 | 同定时器多轴联动 | 547 | Arcaea Lite ✅ |
| 博客 | FreeRTOS 任务通知链 | 536 | Arcaea Lite ✅ |
| 博客 | 双工具链构建 | 537 | Arcaea Lite ✅ |
| 博客 | 软件 I2C + EEPROM BSP | 538 | Arcaea Lite ✅ |
| 博客 | 状态机引擎内部 | 539 | Arcaea Lite ✅ |
| 博客 | 步进 S 曲线 done 置位 | 540 | Arcaea Lite ✅ |
| 博客 | BSP 实现总览 | 541 | Arcaea Lite ✅ |
| 博客 | HMIS 协议取代 DGUS | 542 | Arcaea Lite ✅ V1.1 |
| 博客 | rcfg 参数框架内部 | 543 | Arcaea Lite ✅ |

### 页面状态扫描（2026-05-28, v5.4 修复后）

<<<<<<< HEAD
| 元素 | Games (375) | Music (430) |
|------|------------|-------------|
| Wrapper 类名 | `.games-arcaea-wrap` | `.games-arcaea-wrap`（已统一） |
| Overlay 类名 | `.bg-overlay` | `.bg-overlay`（已统一） |
| :root 覆盖 | ✅ | ✅（新增） |
| bg-glow ×2 | ✅ | ✅（统一） |
| hero-right sticky | ❌ 已移除（v5.4 修复） | ❌ 已移除 |
| blockquote !important | ✅ | ✅（新增） |
| Mood Spectrum | ✅ | ✅（新增） |
| Artist Grid | N/A | ✅ |
| Resonance | N/A | ✅ |
| Prism 玻璃代码块 | ✅（全局） | ✅（全局） |
| LightGallery | ✅（全局） | ✅（全局） |
=======
| 约定 | 说明 |
|------|------|
| 主分类 | 13 = 嵌入式实战 |
| 主文章保留 | 全景概览不改，详细小节拆为独立教程 |
| 交叉引用 | 主文末尾加教程链接，教程末尾加回主文 |
| 标签 | 每篇文章自动打 2-3 个标签（HPM/STM32/OpenOCD/调试等） |
| 代码高亮 | Prism.js 玻璃卡片风格（已全局） |
| 表格 | 用 pipe table，发布前验证 <wp-block-table> 已正确渲染 |
| 图片 | LightGallery 配置已全局生效（.entry-content img） |

### 已知博客内容目录（2026-05-29 全站统一后）

| 类型 | 页面/文章 | ID | 风格 |
|------|----------|----|------|
| 游戏展示 | Games 页面（🎮 游戏） | 375 | Arcaea 玻璃卡片 v5.3 |
| 音乐展示 | Music 页面（🌌 音乐） | 430 | Arcaea 玻璃卡片 v5.3 |
| 全站文章 | 43 篇文章 | 全量 | **统一 Arcaea Article Wrapper v1.6.0** |
| 分类页 | 架构与重构 / 工程复盘 / 嵌入式专题 / 学习记录 / 方法与工具 | 各页面 | 纯 Gutenberg（尚未包裹） |

**全站状态（2026-05-29）**：
- ✅ 所有 43 篇文章已统一 Arcaea article-wrapper CSS
- ✅ 英文文章 ID 780 已翻译为中文
- ✅ 杂乱内联 style 已清理（移除旧版零散 CSS）
- ✅ hash-in-text 已清除（改用 h3::before）
- ❌ 分类页面（架构/复盘/嵌入式专题等）尚未包裹 Arcaea 风格

以下是对 https://babel36acl.xyz/ 的实际扫描结果（2026-05-28）：

### 首页

| 元素 | 状态 | 说明 |
|------|------|------|
| Prism 玻璃代码块 | ✅ 已部署 | FiraCode Nerd Font + 玻璃背景 `rgba(15,18,22,.72)` + blur(18px) + ice blue 边框 |
| 复制按钮玻璃化 | ✅ 已部署 | `rgba(255,255,255,.08)` 背景 + blur(10px) |
| token 去灰底 | ✅ 已部署 | `.token.operator/.entity/.url` 透明 |
| LightGallery | ✅ 已配置 | 在主题选项或自定义 CSS 中 |
| background_blur | ✅ 首页有 | Sakurairo 内置 `backdrop-filter: saturate(120%) blur(8px)` |
| 字体 | ✅ JetBrainsMono | `.Ubuntu-font` 类应用了 Nerd Font |
| --theme-skin | ⚠️ `#505050` | 灰色，非 Arcaea 深色 `#0a0e18` |
| Arcaea 玻璃卡片 | ❌ 未部署 | 首页无 game-category/game-entry/arcaea-wrap |
| bg-overlay | ❌ 未部署 | 首页无 fixed 遮罩层 |

### Games 页面（Page 375）

**全线已部署 ✅** v5.3（2026-05-28 同步 Music 页风格后）：

| 组件 | 状态 | 说明 |
|------|------|------|
| bg-overlay + blur+brightness+saturate | ✅ 已部署 | `rgba(0,0,0,0.62)` + `blur(20px) brightness(0.68) saturate(60%)` |
| 动态光晕 bg-glow-1/bg-glow-2 | ✅ 新增 v5.3 | `floatGlow 20s/24s ease-in-out infinite` |
| 噪声纹理层 fractalNoise | ✅ 新增 v5.3 | `opacity: 0.4; z-index: -2` |
| Hero 区域（渐变标题 + floating-tag） | ✅ 新增 v5.3 | `clamp(48px,8vw,100px)` 渐变标题 + 8 个浮动标签 |
| section-kicker / title / desc 三段式 | ✅ 新增 v5.3 | 7 个区块各自独立 |
| 色调变体 game-entry--{category} | ✅ 新增 v5.3 | 7 种色调（core/rhythm/narrative/exploration/engineering/flight/darkfantasy） |
| mood-spectrum + mood-tag | ✅ 新增 v5.3 | 8 个情绪标签 + 3 种 hover 变体 |
| game-category（轻容器 0.42） | ✅ v5.2 延续 | `box-shadow: none` |
| game-entry（重卡片 0.82） | ✅ v5.2 延续 | + `backdrop-filter: blur(8px)` |
| h3::before # 标记 | ✅ 延续 | `rgba(255,145,145,0.75)` |
| blockquote 渐变底+蓝左边框 | ✅ 延续 | !important 覆盖 FontAwesome |
| steam-section 重型玻璃 | ✅ 延续 | `blur(16px) saturate(70%)` |
| prefers-reduced-motion | ✅ 新增 v5.3 | 禁用所有动画和 transition |
| 响应式 1024px/768px | ✅ 延续 | hero-right hidden on mobile |

### 核心发现

1. **SKILL.md 的 CSS 值**需要更新为 v5.2 实际值以匹配线上
2. 首页 Arcaea 风格未落地 — 后续可考虑
3. 主题选项自定义 CSS 已包含 Prism/FiraCode/预加载等全局样式
4. LightGallery 配置已应用（`lgZoom, lgHash, selector: .entry-content img`）
## Pitfalls

1. **Sakurairo 主题的 !important 块引用**：padding、FontAwesome 引号图标需用 !important 覆盖，且 `::before` + `::after` 都要处理
2. **wpautop 污染 `<style>`**：WordPress 会在 `<style>` 内插 `<p>` 标签，不影响浏览器解析但 source view 难看。还会在 `.hero-right` 的 `<span>` 间插入 `<br />`——display:block 时无害
3. **暗色模式覆盖**：Sakurairo 暗黑模式通过 `[theme-mode="dark"]` 选择器覆盖颜色，需加 !important
4. **backdrop-filter 性能**：不超过 3 层 blur，否则滚动卡顿。Music 页有 3 层（bg-overlay-music blur:4px + artist-cards blur:16px + album-cards blur:14px）
5. **alpha < 0.04 的边框在深色背景上不可见**：在 `#05070d` 基底上最小可用 alpha 为 0.06
6. **CSS 注释污染 meta**：CSS 注释中的文字可能被 WordPress 提取到 `<meta description>`
7. **MCP 工具在 session 重启后丢失**：需确认 wp_mcp_server.py 进程存活，否则用 Python 凭证提取模式
8. **密码安全**：绝不 `curl -u` 带密码
9. **manage_options 权限限制**：app password 无 manage_options scope，无法通过 `wp-json/wp/v2/settings` 改全局主题选项。全局 `--theme-skin` 和 blockquote 覆盖只能嵌入页面级 `<style>` 块中的 `:root {}`。注意仅影响页面内容区，不控制 header/nav/sidebar
10. **blockquote 覆盖仅限 Arcaea-wrap 内**：非 Arcaea 页面的 blockquote 仍受主题默认 FontAwesome 影响
11. **--theme-skin 未覆盖**：首页 `--theme-skin` 实际 `#505050`（灰）而非 `#0a0e18`（深色）。在主题自定义 CSS 中添加修复
12. **prefers-reduced-motion 缺失**：动画（floatGlow, transitions）必须用 `@media (prefers-reduced-motion: reduce)` 禁用。Music 页 v2.1 遗漏，Games 页 v5.3 已修复
13. **wpautop `<br />` 破坏 flex-wrap 容器**：WordPress 的 wpautop 会在 block 级容器内的 `<span>` 元素之间插入 `<br />`。在 `display: flex; flex-wrap: wrap;` 容器（如 mood-spectrum）中，这些 `<br />` 会覆盖 flex 布局，将所有子项强制拆成单列。修复方法：将所有 `<span>` 元素写在同一行，`</span>` 和 `<span` 之间不留任何空白字符。例如 `<div class="mood-spectrum"><span class="mood-tag">A</span><span class="mood-tag">B</span></div>`。多行结构触发 wpautop 插 `<br />`，单行结构安全。适用：mood-spectrum、tag clouds、任何 flex-wrap + `<span>` 子项的组合。
14. **album-wall `minmax` 过大**：在 1380px 宽容器中，`minmax(200px, 1fr)` 产生 5-6 列 × 200px+ 的 1:1 卡片，体积过大。校准值：`minmax(110px, 1fr)`, `gap: 12px`, `padding: 10px`, `border-radius: 12px`。卡片内文字缩小：title `0.78em`, artist `0.68em`, art margin-bottom `8px`。
15. **mood-tag 紧凑尺寸**：大型 mood-spectrum（8+ 标签）需缩小以避免过多换行：`padding: 6px 14px`, `font-size: 0.85em`, `gap: 8px`。必须加 `white-space: nowrap` 防止标签内文字折行。
>>>>>>> 8fcf6a2 (v1.6.0: 全站文章统一 Arcaea 样式 + 英文文章翻译)
