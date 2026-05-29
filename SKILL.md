---
name: sakurairo-arcaea-blog-skill
description: 应用 Arcaea/音游玻璃拟态风格 Sakurairo WordPress 博客的组合技能。封装了多轮迭代设计的精确 CSS 数值、配色 Token、层级体系、Sakurairo 主题冲突覆盖方案和 WordPress MCP 发布工作流。一个文件解决全部。
version: 1.11.0
---

# Sakurairo Arcaea Blog Skill

## 目的

在 Sakurairo 主题的 WordPress 博客上应用 Arcaea 风格（玻璃拟态、深色卡片层级、冰蓝配色）的页面设计、美化和发布。一个文件包含完整工作流：CSS 设计体系、主题冲突覆盖、文章包装、批量样式统一和 WordPress 发布。

## 何时使用

- 用户要求在 Sakurairo 主题博客上"美化页面"、"应用 Arcaea 风格"、"设计 xx 页面"
- 需要创建带玻璃卡片层级的新页面（游戏/音乐展示风格）
- 需要保持跨页面一致的 Arcaea 视觉语言
- 出现 Sakurairo 主题 CSS 冲突（blockquote、FontAwesome 图标等）
- 需要将文章/页面发布到 WordPress
- 需要在文章中使用 Mermaid 图表或 Prism 代码高亮

---

## 目录

- [前置技能加载](#前置技能加载必须)
- [CSS 设计体系](#css-设计体系)
- [文章包裹 CSS](#文章包裹-cssarticle-wrapper--v160-新增)
- [Mermaid 图表 CSS](#mermaid-图表-css)
- [Prism 代码块增强 CSS](#prism-代码块增强-css)
- [配色体系](#配色体系)
- [视觉层级体系](#视觉层级体系)
- [最终校准 CSS 值](#最终校准-css-值v54--games--music-统一)
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

---

## 文章包裹 CSS（Article Wrapper — v1.6.0 新增）

全站 43 篇文章统一 Arcaea 风格的精简 CSS 模板。与 Games/Music 页面的完整页面风格不同，文章包裹 CSS 专注于**阅读体验**：
- 代码块毛玻璃（FiraCode + blur(18px)），覆盖全部 `<pre>` 变体
- Blockquote 渐变底 + 冰蓝左边框（覆盖 Sakurairo FontAwesome）
- h2 底部细线分隔，h3 `#::before` 标记
- 表格暗玻璃卡片样式
- 行内 code 半透明蓝底 + 冰蓝文字色
- `prefers-reduced-motion` 暗模式适配

**完整 CSS 模板文件**：`references/article-wrapper-css.md`
**设计 Token 参考**：`references/visual-tokens.md`

**压缩版（直接用于文章）**：
```css
:root{--arcaea-bg:rgba(8,21,42,0.42);--arcaea-border:rgba(230,238,255,0.78);--arcaea-primary:rgba(238,244,255,0.96);--arcaea-accent:#9db4ff;--arcaea-text:rgba(238,244,255,0.94);--arcaea-muted:rgba(238,244,255,0.65);--arcaea-hash:rgba(255,130,130,0.55)}.arcaea-article-content{position:relative;z-index:1;color:var(--arcaea-text);max-width:100%}.arcaea-article-content h2{color:rgba(238,244,255,0.96);font-size:1.65em;font-weight:700;margin-top:2em;margin-bottom:0.6em;padding-bottom:0.3em;border-bottom:1px solid rgba(230,238,255,0.40);text-shadow:0 2px 10px rgba(0,0,0,0.45)}.arcaea-article-content h3{display:flex;align-items:center;gap:10px;color:rgba(238,244,255,0.96);font-size:1.35em;font-weight:700;margin-top:1.5em;margin-bottom:0.5em;text-shadow:0 2px 10px rgba(0,0,0,0.45)}.arcaea-article-content h3::before{content:"#";color:var(--arcaea-hash);font-size:0.9em;font-weight:700;flex-shrink:0}.arcaea-article-content h2::after,.arcaea-article-content h3::after{display:none!important}.arcaea-article-content p{line-height:1.8;margin:1em 0;color:rgba(238,244,255,0.94)}.arcaea-article-content pre,.arcaea-article-content pre.wp-block-preformatted,.arcaea-article-content pre.arcaea-code,.arcaea-article-content pre[class*="language-"]{font-family:"FiraCode Nerd Font","Fira Code",Consolas,monospace!important;font-size:15px;line-height:1.7;background:rgba(8,21,42,0.42)!important;color:rgba(238,244,255,0.94)!important;border:1px solid rgba(230,238,255,0.78);border-radius:10px;backdrop-filter:blur(12px) saturate(130%);-webkit-backdrop-filter:blur(12px) saturate(130%);box-shadow:0 12px 36px rgba(0,0,0,0.22),inset 0 1px 0 rgba(255,255,255,0.12);padding:1.35rem 1.5rem;margin:2rem 0;overflow:auto}.arcaea-article-content code{font-family:"FiraCode Nerd Font","Fira Code",Consolas,monospace!important;background:rgba(230,238,255,0.10);padding:0.2em 0.4em;border-radius:4px;font-size:0.9em;color:rgba(238,244,255,0.94)!important}.arcaea-article-content pre code{background:transparent;padding:0;border-radius:0;font-size:inherit;color:inherit!important}.arcaea-article-content blockquote{background:rgba(8,21,42,0.42)!important;border:1px solid rgba(230,238,255,0.78)!important;border-left:3px solid rgba(230,238,255,0.90)!important;border-radius:10px!important;padding:14px 20px!important;margin:14px 0!important;color:rgba(238,244,255,0.94)!important;box-shadow:0 12px 36px rgba(0,0,0,0.22),inset 0 1px 0 rgba(255,255,255,0.12)!important;backdrop-filter:blur(12px) saturate(130%);-webkit-backdrop-filter:blur(12px) saturate(130%)}.arcaea-article-content blockquote::before{display:none!important;content:none!important}.arcaea-article-content blockquote::after{display:none!important;content:none!important}.arcaea-article-content table{border-collapse:collapse;width:100%;margin:1.5em 0;background:rgba(8,21,42,0.42);border:1px solid rgba(230,238,255,0.78);border-radius:10px;overflow:hidden;backdrop-filter:blur(12px) saturate(130%);-webkit-backdrop-filter:blur(12px) saturate(130%);box-shadow:0 12px 36px rgba(0,0,0,0.22),inset 0 1px 0 rgba(255,255,255,0.12)}.arcaea-article-content th,.arcaea-article-content td{padding:10px 14px;border:1px solid rgba(230,238,255,0.20);text-align:left}.arcaea-article-content th{background:rgba(139,167,255,0.12);color:rgba(238,244,255,0.96);font-weight:600}.arcaea-article-content ul,.arcaea-article-content ol{padding-left:1.5em;margin:0.8em 0}.arcaea-article-content li{margin:0.4em 0;line-height:1.7;color:rgba(238,244,255,0.92);font-weight:600}.arcaea-article-content a{color:#8ad8ff;text-decoration:none;border-bottom:1px solid rgba(138,216,255,0.25);transition:border-color 0.2s}.arcaea-article-content a:hover{border-bottom-color:rgba(138,216,255,0.6)}.arcaea-article-content hr{border:none;height:1px;background:linear-gradient(90deg,transparent,rgba(230,238,255,0.30),transparent);margin:2em 0}.arcaea-article-content img{border-radius:10px;max-width:100%;height:auto}.arcaea-article-content figure{margin:1.5em 0}.arcaea-article-content figcaption{text-align:center;font-size:0.85em;color:var(--arcaea-muted);margin-top:0.5em}.arcaea-article-content .wp-block-heading{color:inherit}.arcaea-article-content .wp-block-paragraph{color:inherit}.arcaea-article-content .wp-block-table{overflow-x:auto}.arcaea-article-content .wp-block-group{background:rgba(8,21,42,0.42);border:1px solid rgba(230,238,255,0.78);border-radius:10px;padding:16px;margin:1.5em 0;backdrop-filter:blur(12px) saturate(130%);-webkit-backdrop-filter:blur(12px) saturate(130%);box-shadow:0 12px 36px rgba(0,0,0,0.22),inset 0 1px 0 rgba(255,255,255,0.12)}@media(prefers-reduced-motion:reduce){.arcaea-article-content *{animation:none!important;transition:none!important}}
```

---

## Mermaid 图表 CSS

Mermaid 图表渲染容器、SVG 自适应缩放、全屏预览。用于在文章中嵌入流程图、时序图、类图等。

**完整 CSS** 见 `references/article-wrapper-css.md` 的「Mermaid 图表 CSS」章节。

### 容器层级

```
.arcaea-mermaid-box（玻璃拟态容器，overflow-x: auto）
  └─ .arcaea-mermaid-diagram（flex 居中，fit-content）
       └─ svg（自动尺寸，无 max-width 限制）
  └─ .arcaea-mermaid-fullscreen-btn（点击全屏）

.arcaea-mermaid-overlay（fixed 全屏覆层，blur 背景）
  └─ .arcaea-mermaid-overlay-content（居中容器）
  └─ .arcaea-mermaid-overlay-close（关闭按钮，ESC 键支持）
```

### 关键设计要点

1. **SVG 不自缩**：`max-width: none; width: auto; height: auto` 让 SVG 保持原始分辨率，不随容器缩小。超宽图由父容器 `overflow-x: auto` 滚动
2. **小图居中**：`.arcaea-mermaid-diagram` 用 `display: flex; justify-content: center` + `min-width: fit-content`
3. **全屏按钮**：hover 时出现（`opacity: 0 → 1`），点击打开全屏覆层
4. **全屏覆层**：`fixed` 定位 + `backdrop-filter: blur(24px)` + `z-index: 999999`。支持点击背景关闭和 ESC 关闭

### 引用文件

- 完整 Mermaid CSS：`references/article-wrapper-css.md`
- 注入模式：`references/mermaid-injection-pattern.md`
- Prism/Mermaid 冲突：`references/prism-mermaid-conflict.md`

---

## Prism 代码块增强 CSS

Prism.js 代码块的行号、工具栏（语言标签 + 复制按钮）、括号匹配、Previewers 玻璃拟态层、语法着色。

**完整 CSS** 见 `references/article-wrapper-css.md` 的「Prism 代码块增强 CSS」章节。

### 功能列表

| 功能 | CSS Selector | 说明 |
|------|-------------|------|
| **行号** | `pre.line-numbers` | 行号列宽 3.2em，padding-left 4.1em，行号颜色 rgba(238,244,255,0.38) |
| **工具栏** | `div.code-toolbar > .toolbar` | 右上角定位，毛玻璃按钮，语言标签 + 复制按钮 |
| **复制按钮** | `.copy-to-clipboard-button` | 圆角 999px，冰蓝边框，hover 增强 |
| **语言标签** | `.toolbar-item span` | 显示代码语言名称（如 "JavaScript"、"Plain text"） |
| **语法着色** | `.token.*` | Arcaea Dark 配色（橙色/绿/天蓝/紫/黄） |
| **行高亮** | `.line-highlight` | 渐变色高亮 + 左侧冰蓝竖线 |
| **命令行提示符** | `.command-line-prompt` | `>` 提示符，天蓝色 |
| **自定义滚动条** | `::-webkit-scrollbar` | 6px 宽，冰蓝圆角滑块 |

### 语法着色配色

| Token | 颜色 | 示例 |
|-------|------|------|
| comment/prolog/doctype/cdata | `rgba(238,244,255,0.48)` 斜体 | `// 注释` |
| property/tag/boolean/number | `#ffc28a` 橙色 | `123`, `true` |
| selector/attr-name/string | `#b8f0d0` 绿色 | `"hello"`, `.class` |
| atrule/attr-value/keyword | `#8ad8ff` 天蓝 | `import`, `function` |
| function/class-name | `#c7b6ff` 淡紫 | `myFunc()`, `MyClass` |
| regex/important/variable | `#ffe6a8` 黄色 | `/regex/`, `$var` |

### 引用文件

- 完整 Prism 代码块 CSS：`references/article-wrapper-css.md`
- Previewers Arcaea 玻璃层：`references/previewers-arcaea-layer.md`

---

## 配色体系

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

---

## 视觉层级体系

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

---

## 最终校准 CSS 值（v5.4 — Games + Music 统一）

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
.game-entry--core { background: linear-gradient(145deg, rgba(139,167,255,0.10), rgba(219,234,254,0.04)); border-color: rgba(139,167,255,0.18); }
.game-entry--rhythm { background: linear-gradient(145deg, rgba(177,140,255,0.10), rgba(199,160,255,0.04)); border-color: rgba(177,140,255,0.18); }
.game-entry--narrative { background: linear-gradient(145deg, rgba(100,140,255,0.08), rgba(80,120,230,0.03)); border-color: rgba(100,140,255,0.16); }
.game-entry--exploration { background: linear-gradient(145deg, rgba(120,210,255,0.08), rgba(160,230,255,0.03)); border-color: rgba(120,210,255,0.16); }
.game-entry--engineering { background: linear-gradient(145deg, rgba(150,160,180,0.07), rgba(130,140,160,0.03)); border-color: rgba(150,160,180,0.14); }
.game-entry--flight { background: linear-gradient(145deg, rgba(255,160,80,0.07), rgba(255,180,100,0.03)); border-color: rgba(255,160,80,0.14); }
.game-entry--darkfantasy { background: linear-gradient(145deg, rgba(255,80,100,0.07), rgba(200,60,80,0.03)); border-color: rgba(255,80,100,0.14); }

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

### Section 通用布局

```
.section-kicker — 细微分类标签 (uppercase, violet-tinted)
.section-title  — H2 标题 (text-shadow 紫辉)
.section-desc   — 段落描述 (淡蓝灰)
```

### Blockquote 设计（Sakurairo 覆盖 — 必须 !important）

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

见 [`references/arcaea-lite-wrapper.md`](references/arcaea-lite-wrapper.md)。用于关于、工具箱、嵌入式专题、游记、**博客文章**等非内容密集型页面。带 `bg-glow` 光晕氛围但**无 bg-overlay 模糊层**（避免覆盖正文可读性）。

---

## Sakurairo 主题集成

### CSS 变量覆盖

Sakurairo 主题的颜色 CSS 变量可通过主题选项的自定义 CSS 覆盖：

```css
:root {
  --theme-skin: #0a0e18;              /* 覆盖主题主色为深色底 */
  --theme-skin-dark: #05070d;          /* 暗色模式更深 */
  --global-font-weight: 300;           /* 细体，符合 Arcaea 空灵感 */
}
```

部署方法：在 Sakurairo 主题选项 → 全局设置 → 自定义样式中添加上述 CSS。检查方法：查看首页 HTML 中 `--theme-skin` 的值。

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

- **站点**: 你的 WordPress 站点 URL
- **主题**: Sakurairo（推荐 v3.0.10+）
- **MCP 服务器**: `~/.hermes/scripts/wp_mcp_server.py`（确保进程运行中）
- **凭证**: 不在此文件中记录。MCP 脚本从环境变量或自身源码读取

### 可用 MCP 工具

1. `GET /wp-json/wp/v2/pages/{id}?context=edit` 获取 `content.raw`（原始 HTML，未被 wpautop 污染）
2. 在 Python 中修改 `raw` 内容（精确字符串匹配）
3. `POST /wp-json/wp/v2/pages/{id}` 传回 `{"content": modified_raw}`
4. 用 `curl -sL URL | grep` 验证关键 CSS 选择器存在

**注意：MCP 工具没有 delete 操作。** 需要删除文章时，通过 Python REST API 的 `method="DELETE"` 移入回收站：

```python
req = urllib.request.Request(f"{SITE}/wp-json/wp/v2/posts/{post_id}",
    method="DELETE", headers=HEADERS)
```

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
