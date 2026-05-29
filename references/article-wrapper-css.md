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

## 配色说明

| Token | Value | 用途 |
|-------|-------|------|
| --arcaea-bg | `rgba(8,21,42,0.42)` | 卡片深蓝半透明底 |
| --arcaea-border | `rgba(230,238,255,0.78)` | 亮白细边框 |
| --arcaea-primary | `rgba(238,244,255,0.96)` | 标题/强调文字 |
| --arcaea-text | `rgba(238,244,255,0.94)` | 正文 |
| --arcaea-muted | `rgba(238,244,255,0.65)` | 辅助文字 |
| --arcaea-hash | `rgba(255,130,130,0.55)` | 标题 # 标记 |

核心样式：`backdrop-filter: blur(12px) saturate(130%)` + `box-shadow: ... inset 0 1px 0 rgba(255,255,255,0.12)`（白色内嵌高光边）。
