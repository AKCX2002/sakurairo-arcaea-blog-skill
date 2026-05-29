# Arcaea Article Wrapper CSS

统一 Arcaea 玻璃卡片风格。深色半透明背景 + 亮白细边框 + 高对比文字 + 轻微毛玻璃。

## 使用方式

每篇文章内容以以下结构开头：

```html
<style>
/* 此处放压缩后的 CSS */
</style>
<div class="arcaea-article-content">

<!-- 原始文章内容 -->

</div>
```

## 压缩版 CSS（用于 WordPress 文章）

```css
:root{--arcaea-bg:rgba(8,21,42,0.42);--arcaea-border:rgba(230,238,255,0.78);--arcaea-primary:rgba(238,244,255,0.96);--arcaea-accent:#9db4ff;--arcaea-text:rgba(238,244,255,0.94);--arcaea-muted:rgba(238,244,255,0.65);--arcaea-hash:rgba(255,130,130,0.55)}.arcaea-article-content{position:relative;z-index:1;color:var(--arcaea-text);max-width:100%}.arcaea-article-content h2{color:rgba(238,244,255,0.96);font-size:1.65em;font-weight:700;margin-top:2em;margin-bottom:0.6em;padding-bottom:0.3em;border-bottom:1px solid rgba(230,238,255,0.40);text-shadow:0 2px 10px rgba(0,0,0,0.45)}.arcaea-article-content h3{display:flex;align-items:center;gap:10px;color:rgba(238,244,255,0.96);font-size:1.35em;font-weight:700;margin-top:1.5em;margin-bottom:0.5em;text-shadow:0 2px 10px rgba(0,0,0,0.45)}.arcaea-article-content h3::before{content:"#";color:var(--arcaea-hash);font-size:0.9em;font-weight:700;flex-shrink:0}.arcaea-article-content h2::after,.arcaea-article-content h3::after{display:none!important}.arcaea-article-content p{line-height:1.8;margin:1em 0;color:rgba(238,244,255,0.94)}.arcaea-article-content pre,.arcaea-article-content pre.wp-block-preformatted,.arcaea-article-content pre.arcaea-code,.arcaea-article-content pre[class*="language-"]{font-family:"FiraCode Nerd Font","Fira Code",Consolas,monospace!important;font-size:15px;line-height:1.7;background:rgba(8,21,42,0.42)!important;color:rgba(238,244,255,0.94)!important;border:1px solid rgba(230,238,255,0.78);border-radius:10px;backdrop-filter:blur(12px) saturate(130%);-webkit-backdrop-filter:blur(12px) saturate(130%);box-shadow:0 12px 36px rgba(0,0,0,0.22),inset 0 1px 0 rgba(255,255,255,0.12);padding:1.35rem 1.5rem;margin:2rem 0;overflow:auto}.arcaea-article-content code{font-family:"FiraCode Nerd Font","Fira Code",Consolas,monospace!important;background:rgba(230,238,255,0.10);padding:0.2em 0.4em;border-radius:4px;font-size:0.9em;color:rgba(238,244,255,0.94)!important}.arcaea-article-content pre code{background:transparent;padding:0;border-radius:0;font-size:inherit;color:inherit!important}.arcaea-article-content blockquote{background:rgba(8,21,42,0.42)!important;border:1px solid rgba(230,238,255,0.78)!important;border-left:3px solid rgba(230,238,255,0.90)!important;border-radius:10px!important;padding:14px 20px!important;margin:14px 0!important;color:rgba(238,244,255,0.94)!important;box-shadow:0 12px 36px rgba(0,0,0,0.22),inset 0 1px 0 rgba(255,255,255,0.12)!important;backdrop-filter:blur(12px) saturate(130%);-webkit-backdrop-filter:blur(12px) saturate(130%)}.arcaea-article-content blockquote::before{display:none!important;content:none!important}.arcaea-article-content blockquote::after{display:none!important;content:none!important}.arcaea-article-content table{border-collapse:collapse;width:100%;margin:1.5em 0;background:rgba(8,21,42,0.42);border:1px solid rgba(230,238,255,0.78);border-radius:10px;overflow:hidden;backdrop-filter:blur(12px) saturate(130%);-webkit-backdrop-filter:blur(12px) saturate(130%);box-shadow:0 12px 36px rgba(0,0,0,0.22),inset 0 1px 0 rgba(255,255,255,0.12)}.arcaea-article-content th,.arcaea-article-content td{padding:10px 14px;border:1px solid rgba(230,238,255,0.20);text-align:left}.arcaea-article-content th{background:rgba(139,167,255,0.12);color:rgba(238,244,255,0.96);font-weight:600}.arcaea-article-content ul,.arcaea-article-content ol{padding-left:1.5em;margin:0.8em 0}.arcaea-article-content li{margin:0.4em 0;line-height:1.7;color:rgba(238,244,255,0.92);font-weight:600}.arcaea-article-content a{color:#8ad8ff;text-decoration:none;border-bottom:1px solid rgba(138,216,255,0.25);transition:border-color 0.2s}.arcaea-article-content a:hover{border-bottom-color:rgba(138,216,255,0.6)}.arcaea-article-content hr{border:none;height:1px;background:linear-gradient(90deg,transparent,rgba(230,238,255,0.30),transparent);margin:2em 0}.arcaea-article-content img{border-radius:10px;max-width:100%;height:auto}.arcaea-article-content figure{margin:1.5em 0}.arcaea-article-content figcaption{text-align:center;font-size:0.85em;color:var(--arcaea-muted);margin-top:0.5em}.arcaea-article-content .wp-block-heading{color:inherit}.arcaea-article-content .wp-block-paragraph{color:inherit}.arcaea-article-content .wp-block-table{overflow-x:auto}.arcaea-article-content .wp-block-group{background:rgba(8,21,42,0.42);border:1px solid rgba(230,238,255,0.78);border-radius:10px;padding:16px;margin:1.5em 0;backdrop-filter:blur(12px) saturate(130%);-webkit-backdrop-filter:blur(12px) saturate(130%);box-shadow:0 12px 36px rgba(0,0,0,0.22),inset 0 1px 0 rgba(255,255,255,0.12)}@media(prefers-reduced-motion:reduce){.arcaea-article-content *{animation:none!important;transition:none!important}}
```

## Mermaid 图表 CSS

Mermaid 图表渲染容器、SVG 自适应缩放、全屏预览。

```css
/* ── Mermaid 外层包裹容器 ──
 * 玻璃拟态背景、横向滚动、悬停动效
 * 小图 → 无滚动，SVG 居中
 * 大图 → 横向滚动条出现，不挤压 SVG
 */
.arcaea-mermaid-box {
  position: relative;
  background: rgba(20, 25, 35, 0.35);
  -webkit-backdrop-filter: blur(16px);
  backdrop-filter: blur(16px);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 18px;
  box-shadow:
    0 0 20px rgba(120, 220, 255, 0.08),
    0 0 40px rgba(120, 220, 255, 0.03),
    0 10px 30px rgba(0, 0, 0, 0.18);
  margin: 2rem 0;
  padding: 1rem;
  overflow-x: auto;
  overflow-y: hidden;
  -webkit-transition: -webkit-transform 0.25s ease, box-shadow 0.25s ease, border-color 0.25s ease;
  transition: transform 0.25s ease, box-shadow 0.25s ease, border-color 0.25s ease;
  overscroll-behavior: contain;
}

.arcaea-mermaid-box:hover {
  transform: scale(1.01);
  border-color: rgba(255, 255, 255, 0.14);
  box-shadow:
    0 0 25px rgba(120, 220, 255, 0.12),
    0 0 50px rgba(120, 220, 255, 0.05),
    0 12px 36px rgba(0, 0, 0, 0.22);
}

.arcaea-mermaid-box::before {
  display: none !important;
  content: none !important;
}

/* ── Mermaid 图表内部容器 ── */
.arcaea-mermaid-diagram,
.mermaid.arcaea-mermaid-diagram {
  min-width: fit-content;
  min-width: -moz-fit-content;
  display: flex;
  justify-content: center;
  margin: 0.5rem 0;
  color: rgba(238, 244, 255, 0.94);
  font-family:
    "FiraCode Nerd Font",
    "Fira Code",
    "JetBrains Mono",
    "Noto Sans SC",
    "Microsoft YaHei",
    sans-serif;
}

/* ── SVG 基本样式 ── */
.arcaea-mermaid-diagram svg {
  display: block;
  max-width: none !important;
  width: auto !important;
  height: auto !important;
  margin: 0 auto;
  padding: 14px;
  box-sizing: content-box;
  background:
    linear-gradient(180deg,
      rgba(15, 24, 42, 0.32),
      rgba(8, 16, 32, 0.20));
  border: 1px solid rgba(230, 238, 255, 0.28);
  border-radius: 14px;
  box-shadow:
    inset 0 1px 0 rgba(255, 255, 255, 0.08);
}

/* ── 文字样式 ── */
.arcaea-mermaid-diagram svg text,
.arcaea-mermaid-diagram .label,
.arcaea-mermaid-diagram .nodeLabel,
.arcaea-mermaid-diagram .edgeLabel,
.arcaea-mermaid-diagram .actor,
.arcaea-mermaid-diagram .messageText,
.arcaea-mermaid-diagram .noteText {
  fill: rgba(238, 244, 255, 0.94) !important;
  color: rgba(238, 244, 255, 0.94) !important;
  font-size: 15px !important;
  font-weight: 500;
  font-family:
    "FiraCode Nerd Font",
    "Fira Code",
    "JetBrains Mono",
    "Noto Sans SC",
    "Microsoft YaHei",
    sans-serif !important;
}

/* ── 节点边框 ── */
.arcaea-mermaid-diagram .node rect,
.arcaea-mermaid-diagram .node circle,
.arcaea-mermaid-diagram .node ellipse,
.arcaea-mermaid-diagram .node polygon,
.arcaea-mermaid-diagram .node path {
  stroke: rgba(160, 220, 255, 0.45) !important;
  stroke-width: 1.6px !important;
}

/* ── 连线 ── */
.arcaea-mermaid-diagram .edgePath path,
.arcaea-mermaid-diagram .flowchart-link {
  stroke: rgba(160, 220, 255, 0.55) !important;
  stroke-width: 1.8px !important;
}

/* ── 箭头标记 ── */
.arcaea-mermaid-diagram marker path,
.arcaea-mermaid-diagram marker polygon,
.arcaea-mermaid-diagram marker circle {
  fill: rgba(160, 220, 255, 0.55) !important;
  stroke: rgba(160, 220, 255, 0.55) !important;
}

/* ── 集群盒子 ── */
.arcaea-mermaid-diagram .cluster rect {
  fill: transparent !important;
  stroke: rgba(141, 199, 255, 0.70) !important;
}

/* ── 错误状态 ── */
.arcaea-mermaid-box.arcaea-mermaid-error {
  border-color: rgba(255, 120, 160, 0.38);
  background:
    linear-gradient(135deg,
      rgba(255, 95, 140, 0.10),
      rgba(120, 180, 255, 0.06));
}

.arcaea-mermaid-error-message {
  margin-top: 0.85rem;
  padding: 0.75rem 0.95rem;
  border-radius: 12px;
  color: #ffd9e5;
  background: rgba(80, 20, 42, 0.55);
  border: 1px solid rgba(255, 150, 185, 0.25);
  font-size: 0.92rem;
  line-height: 1.6;
}

/* ── 全屏预览按钮 ── */
.arcaea-mermaid-fullscreen-btn {
  position: absolute;
  top: 8px;
  right: 8px;
  z-index: 10;
  display: flex;
  align-items: center;
  justify-content: center;
  width: 32px;
  height: 32px;
  border: 1px solid rgba(255, 255, 255, 0.15);
  border-radius: 8px;
  background: rgba(20, 25, 35, 0.6);
  -webkit-backdrop-filter: blur(8px);
  backdrop-filter: blur(8px);
  color: rgba(238, 244, 255, 0.7);
  font-size: 16px;
  cursor: pointer;
  opacity: 0;
  -webkit-transition: opacity 0.2s ease, background 0.2s ease;
  transition: opacity 0.2s ease, background 0.2s ease;
}

.arcaea-mermaid-box:hover .arcaea-mermaid-fullscreen-btn {
  opacity: 1;
}

.arcaea-mermaid-fullscreen-btn:hover {
  background: rgba(40, 50, 70, 0.8);
  color: rgba(238, 244, 255, 1);
}

/* ── 全屏覆层 ── */
.arcaea-mermaid-overlay {
  position: fixed;
  inset: 0;
  z-index: 999999;
  display: flex;
  align-items: center;
  justify-content: center;
  background: rgba(10, 12, 18, 0.92);
  -webkit-backdrop-filter: blur(24px);
  backdrop-filter: blur(24px);
  opacity: 0;
  pointer-events: none;
  -webkit-transition: opacity 0.3s ease;
  transition: opacity 0.3s ease;
}

.arcaea-mermaid-overlay.active {
  opacity: 1;
  pointer-events: auto;
}

.arcaea-mermaid-overlay .arcaea-mermaid-overlay-content {
  max-width: 94vw;
  max-height: 92vh;
  overflow: auto;
  border-radius: 18px;
  background: rgba(20, 25, 35, 0.5);
  padding: 1rem;
}

.arcaea-mermaid-overlay .arcaea-mermaid-overlay-content svg {
  max-width: none !important;
  width: auto !important;
  height: auto !important;
  display: block;
  margin: 0 auto;
}

.arcaea-mermaid-overlay-close {
  position: absolute;
  top: 20px;
  right: 24px;
  width: 40px;
  height: 40px;
  display: flex;
  align-items: center;
  justify-content: center;
  border: 1px solid rgba(255, 255, 255, 0.2);
  border-radius: 10px;
  background: rgba(20, 25, 35, 0.7);
  -webkit-backdrop-filter: blur(8px);
  backdrop-filter: blur(8px);
  color: rgba(238, 244, 255, 0.85);
  font-size: 20px;
  cursor: pointer;
  transition: background 0.2s ease;
}

.arcaea-mermaid-overlay-close:hover {
  background: rgba(50, 60, 85, 0.8);
}
```

## Prism 代码块增强 CSS

Prism.js 行号、工具栏（语言标签 + 复制按钮）、括号匹配、Previewers 玻璃拟态层、语法着色。

```css
/* ── 代码块 CSS 变量 ── */
:root {
  --bac-code-font: "FiraCode Nerd Font", "Fira Code", "JetBrains Mono", Consolas, monospace;
  --bac-code-radius: 10px;
  --bac-code-blur: blur(12px) saturate(130%);
  --bac-code-shadow: 0 12px 36px rgba(0, 0, 0, 0.22), inset 0 1px 0 rgba(255, 255, 255, 0.12);
}

/* ── 代码块基础 ── */
pre[class*="language-"],
.arcaea-article-content pre[class*="language-"] {
  position: relative;
  margin: 2rem 0 !important;
  padding: 1.35rem 1.5rem !important;
  overflow: auto;
  border-radius: var(--bac-code-radius) !important;
  font-family: var(--bac-code-font) !important;
  font-size: 15px !important;
  line-height: 1.7 !important;
  text-shadow: none !important;
  tab-size: 4;
  -webkit-backdrop-filter: var(--bac-code-blur);
  backdrop-filter: var(--bac-code-blur);
  box-shadow: var(--bac-code-shadow) !important;
}

code[class*="language-"],
pre[class*="language-"] code,
.arcaea-article-content pre code[class*="language-"] {
  font-family: var(--bac-code-font) !important;
  font-size: inherit !important;
  line-height: inherit !important;
  text-shadow: none !important;
  background: transparent !important;
  white-space: pre;
}

:not(pre) > code[class*="language-"] {
  border-radius: 4px;
  padding: 0.2em 0.4em;
  font-family: var(--bac-code-font) !important;
  font-size: 0.9em;
}

/* ── 行号（Line Numbers）── */
pre.line-numbers,
.arcaea-article-content pre.line-numbers,
.arcaea-article-content pre[class*="language-"].line-numbers {
  padding-left: 4.1em !important;
}

.line-numbers .line-numbers-rows {
  top: 1.35rem;
  left: 0;
  width: 3.2em;
  border-right-width: 1px;
  font-family: var(--bac-code-font) !important;
  line-height: 1.7 !important;
}

.line-numbers .line-numbers-rows {
  border-right-color: rgba(230, 238, 255, 0.20) !important;
}

.line-numbers-rows > span::before {
  color: rgba(238, 244, 255, 0.38) !important;
}

/* ── 工具栏（语言标签 + 复制按钮）── */
div.code-toolbar {
  position: relative;
}

div.code-toolbar > .toolbar {
  top: 0.55rem;
  right: 0.65rem;
  opacity: 1;
}

div.code-toolbar > .toolbar > .toolbar-item {
  margin-left: 0.35rem;
}

div.code-toolbar > .toolbar button,
div.code-toolbar > .toolbar a,
div.code-toolbar > .toolbar > .toolbar-item > span {
  border-radius: 999px !important;
  padding: 0.28rem 0.62rem !important;
  font-family: var(--bac-code-font) !important;
  font-size: 12px !important;
  line-height: 1.2 !important;
  box-shadow: none !important;
  -webkit-transition: opacity 0.2s ease, border-color 0.2s ease, background-color 0.2s ease;
  color: rgba(238, 244, 255, 0.84) !important;
  background: rgba(8, 12, 20, 0.72) !important;
  border: 1px solid rgba(230, 238, 255, 0.18) !important;
}

div.code-toolbar > .toolbar button:hover,
div.code-toolbar > .toolbar a:hover,
div.code-toolbar > .toolbar > .toolbar-item > span:hover {
  color: rgba(238, 244, 255, 0.96) !important;
  background: rgba(139, 167, 255, 0.16) !important;
  border-color: rgba(138, 216, 255, 0.58) !important;
}

/* ── 语法着色 ── */
pre[class*="language-"] {
  color: rgba(238, 244, 255, 0.94) !important;
  background: rgba(8, 21, 42, 0.42) !important;
  border: 1px solid rgba(230, 238, 255, 0.78) !important;
}

code[class*="language-"],
pre[class*="language-"] code {
  color: rgba(238, 244, 255, 0.94) !important;
}

.token.comment,
.token.prolog,
.token.doctype,
.token.cdata {
  color: rgba(238, 244, 255, 0.48) !important;
  font-style: italic;
}

.token.punctuation,
.token.operator {
  color: rgba(238, 244, 255, 0.76) !important;
}

.token.namespace {
  opacity: 0.72;
}

.token.property,
.token.tag,
.token.boolean,
.token.number,
.token.constant,
.token.symbol,
.token.deleted {
  color: #ffc28a !important;
}

.token.selector,
.token.attr-name,
.token.string,
.token.char,
.token.builtin,
.token.inserted {
  color: #b8f0d0 !important;
}

.token.atrule,
.token.attr-value,
.token.keyword {
  color: #8ad8ff !important;
}

.token.function,
.token.class-name {
  color: #c7b6ff !important;
}

.token.regex,
.token.important,
.token.variable {
  color: #ffe6a8 !important;
}

.token.entity,
.token.url {
  color: #9db4ff !important;
  background: transparent !important;
}

.token.bold,
.token.important {
  font-weight: 700;
}

/* ── 行高亮 ── */
pre[class*="language-"] mark,
pre[class*="language-"] .line-highlight {
  margin-top: 1.35rem;
  border-radius: 6px;
}

pre[class*="language-"] .line-highlight {
  background: linear-gradient(90deg, rgba(138, 216, 255, 0.13), rgba(157, 180, 255, 0.06)) !important;
  box-shadow: inset 3px 0 0 rgba(138, 216, 255, 0.55);
}

/* ── 命令行提示符 ── */
.command-line-prompt {
  border-right-width: 1px;
  margin-right: 1em;
  border-right-color: rgba(230, 238, 255, 0.20) !important;
  color: rgba(138, 216, 255, 0.72) !important;
}

/* ── 不可见字符 ── */
.token.tab:not(:empty)::before,
.token.cr::before,
.token.lf::before,
.token.space::before {
  opacity: 0.45;
}

/* ── 自定义滚动条 ── */
pre[class*="language-"]::-webkit-scrollbar {
  height: 10px;
  width: 10px;
}

pre[class*="language-"]::-webkit-scrollbar-track {
  background: rgba(255, 255, 255, 0.04);
}

pre[class*="language-"]::-webkit-scrollbar-thumb {
  border-radius: 999px;
  border: 2px solid transparent;
  background-clip: content-box;
  background-color: rgba(138, 216, 255, 0.38);
}

pre[class*="language-"]::-webkit-scrollbar-thumb:hover {
  background-color: rgba(138, 216, 255, 0.58);
}

/* ── 减少动效 ── */
@media (prefers-reduced-motion: reduce) {
  pre[class*="language-"],
  div.code-toolbar > .toolbar button,
  div.code-toolbar > .toolbar a,
  div.code-toolbar > .toolbar span {
    transition: none !important;
    animation: none !important;
  }
}
```

## Prism Previewers Arcaea 玻璃层

颜色/渐变/角度/时间/缓动实时预览的玻璃拟态容器。

```css
.prism-previewer {
  margin-top: -56px;
  width: 42px;
  height: 42px;
  margin-left: -21px;
  z-index: 9999;
  -webkit-backdrop-filter: blur(20px) saturate(140%);
  backdrop-filter: blur(20px) saturate(140%);
  border-radius: 16px;
  overflow: visible;
  box-shadow:
    0 8px 32px rgba(0, 0, 0, 0.40),
    0 0 24px rgba(130, 170, 255, 0.18);
  -webkit-transition:
    opacity 0.25s ease,
    -webkit-transform 0.25s ease;
  transition:
    opacity 0.25s ease,
    transform 0.25s ease;
}

.prism-previewer.active:hover {
  transform: scale(1.08);
}

.prism-previewer.flipped {
  margin-top: 0;
  margin-bottom: -56px;
}

.prism-previewer:before {
  top: -2px;
  right: -2px;
  left: -2px;
  bottom: -2px;
  border-radius: 18px;
  border: 1px solid rgba(255, 255, 255, 0.08);
  box-shadow:
    inset 0 0 12px rgba(130, 170, 255, 0.06),
    0 0 0 rgba(130, 170, 255, 0);
}

.prism-previewer:after {
  top: 100%;
  width: 0;
  height: 0;
  margin: 4px 0 0 -8px;
  border: 8px solid transparent;
  border-top-color: rgba(130, 170, 255, 0.35);
  filter: drop-shadow(0 0 4px rgba(130, 170, 255, 0.30));
}

.prism-previewer.flipped:after {
  top: auto;
  bottom: 100%;
  margin-top: 0;
  margin-bottom: 4px;
  border-top-color: transparent;
  border-bottom-color: rgba(130, 170, 255, 0.35);
}
```

## 配色说明

| Token | Value | 用途 |
|-------|-------|------|
| --arcaea-bg | `rgba(8,21,42,0.42)` | 卡片深蓝半透明底 |
| --arcaea-border | `rgba(230,238,255,0.78)` | 亮白细边框 |
| --arcaea-primary | `rgba(238,244,255,0.96)` | 标题/强调文字 |
| --arcaea-text | `rgba(238,244,255,0.94)` | 正文 |
| --arcaea-muted | `rgba(238,244,255,0.65)` | 辅助文字 |
| --arcaea-hash | `rgba(255,130,130,0.55)` | 标题 # 标记 |
| --bac-code-font | FiraCode Nerd Font / Fira Code / JetBrains Mono | 代码字体 |

核心玻璃样式：`backdrop-filter: blur(12px) saturate(130%)` + `box-shadow: ... inset 0 1px 0 rgba(255,255,255,0.12)`（白色内嵌高光边）。
