# Arcaea Article Wrapper CSS

全站文章统一 Arcaea 风格的 CSS 模板。注入到每篇文章内容头部，包裹在 `<div class="arcaea-article-content">` 中。

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
:root{--arcaea-bg:rgba(14,18,28,0.72);--arcaea-border:rgba(160,220,255,0.12);--arcaea-primary:#b0dcff;--arcaea-accent:#9db4ff;--arcaea-text:#f0f6ff;--arcaea-muted:#a0b8d8;--arcaea-hash:rgba(255,130,130,0.55)}.arcaea-article-content{position:relative;z-index:1;color:var(--arcaea-text);max-width:100%}.arcaea-article-content h2{color:#e8f0ff;font-size:1.65em;font-weight:700;margin-top:2em;margin-bottom:0.6em;padding-bottom:0.3em;border-bottom:1px solid rgba(160,220,255,0.15);text-shadow:0 0 10px rgba(180,150,255,0.30),0 2px 4px rgba(0,0,0,0.50)}.arcaea-article-content h3{display:flex;align-items:center;gap:10px;color:#e2f4ff;font-size:1.35em;font-weight:700;margin-top:1.5em;margin-bottom:0.5em;text-shadow:0 0 12px rgba(126,200,255,0.25),0 2px 4px rgba(0,0,0,0.50)}.arcaea-article-content h3::before{content:"#";color:var(--arcaea-hash);font-size:0.9em;font-weight:700;flex-shrink:0}.arcaea-article-content p{line-height:1.8;margin:1em 0;color:#f0f6ff}.arcaea-article-content pre{font-family:"FiraCode Nerd Font","Fira Code",Consolas,monospace!important;font-size:15px;line-height:1.7;background:rgba(15,18,22,0.72)!important;border:1px solid rgba(160,220,255,0.18);border-radius:16px;backdrop-filter:blur(18px);box-shadow:0 8px 24px rgba(0,0,0,0.28),0 0 0 1px rgba(255,255,255,0.02) inset;padding:1.35rem 1.5rem;margin:2rem 0;overflow:auto}.arcaea-article-content code{font-family:"FiraCode Nerd Font","Fira Code",Consolas,monospace!important;background:rgba(160,220,255,0.08);padding:0.2em 0.4em;border-radius:4px;font-size:0.9em}.arcaea-article-content pre code{background:transparent;padding:0;border-radius:0;font-size:inherit}.arcaea-article-content blockquote{background:linear-gradient(135deg,rgba(40,70,120,0.22),rgba(20,30,50,0.14));border-left:3px solid rgba(126,200,255,0.55);border-radius:12px;padding:14px 20px!important;margin:14px 0!important;color:#f5faff!important;box-shadow:none!important}.arcaea-article-content blockquote::before{display:none!important;content:none!important}.arcaea-article-content blockquote::after{display:none!important;content:none!important}.arcaea-article-content table{border-collapse:collapse;width:100%;margin:1.5em 0;background:rgba(10,14,24,0.42);border-radius:12px;overflow:hidden}.arcaea-article-content th,.arcaea-article-content td{padding:10px 14px;border:1px solid rgba(255,255,255,0.06);text-align:left}.arcaea-article-content th{background:rgba(139,167,255,0.10);color:#c8ddff;font-weight:600}.arcaea-article-content ul,.arcaea-article-content ol{padding-left:1.5em;margin:0.8em 0}.arcaea-article-content li{margin:0.4em 0;line-height:1.7}.arcaea-article-content a{color:#8ad8ff;text-decoration:none;border-bottom:1px solid rgba(138,216,255,0.25);transition:border-color 0.2s}.arcaea-article-content a:hover{border-bottom-color:rgba(138,216,255,0.6)}.arcaea-article-content hr{border:none;height:1px;background:linear-gradient(90deg,transparent,rgba(160,220,255,0.2),transparent);margin:2em 0}.arcaea-article-content img{border-radius:12px;max-width:100%;height:auto}.arcaea-article-content figure{margin:1.5em 0}.arcaea-article-content figcaption{text-align:center;font-size:0.85em;color:var(--arcaea-muted);margin-top:0.5em}.arcaea-article-content .wp-block-heading{color:inherit}.arcaea-article-content .wp-block-paragraph{color:inherit}.arcaea-article-content .wp-block-table{overflow-x:auto}@media(prefers-reduced-motion:reduce){.arcaea-article-content *{animation:none!important;transition:none!important}}
```

## 全站批量应用脚本（Python + WordPress REST API）

```python
import os, re, json, urllib.request, base64, urllib.error

SITE = "https://babel36acl.xyz"
# 从 wp_mcp_server.py 中提取凭证，避免明文密码
MCP = os.path.expanduser("~/.hermes/scripts/wp_mcp_server.py")
with open(MCP) as f:
    src = f.read()
s = re.search(r'SITE = os\.environ\.get\("([^"]+)"', src).group(1)
u = re.search(r'USER = os\.environ\.get\("([^"]+)"', src).group(1)
p = re.search(r'PASS = os\.environ\.get\("([^"]+)"', src).group(1)
token = base64.b64encode(f"{u}:{p}".encode()).decode()
HEADERS = {"Authorization": f"Basic {token}", "Content-Type": "application/json"}

ARCAEA_CSS = ""  # 上面压缩版 CSS

def transform_content(content):
    """对单篇文章应用 Arcaea 样式包裹"""
    # 移除已有 style 块
    content = re.sub(r'<style[^>]*>.*?</style>', '', content, flags=re.DOTALL)
    # 移除嵌入的完整 HTML 文档
    content = re.sub(r'<!DOCTYPE[^>]*>', '', content, flags=re.DOTALL)
    content = re.sub(r'<html[^>]*>', '', content, flags=re.DOTALL)
    content = re.sub(r'</html>', '', content, flags=re.DOTALL)
    content = re.sub(r'<head>.*?</head>', '', content, flags=re.DOTALL)
    content = re.sub(r'<body[^>]*>', '', content, flags=re.DOTALL)
    content = re.sub(r'</body>', '', content, flags=re.DOTALL)
    content = re.sub(r'<div\s+class="arcaea-article-content">', '', content)
    content = re.sub(r'</div>\s*$', '', content)
    # 移除 hash-in-text（如果 h3::before 生效，独立 # 不再需要）
    content = re.sub(r'<p>\s*#\s+', '<p>', content)
    content = re.sub(r'<li>\s*#\s+', '<li>', content)
    content = content.strip()
    return f'<style>\n{ARCAEA_CSS}\n</style>\n<div class="arcaea-article-content">\n{content}\n</div>'

# 遍历所有文章
req = urllib.request.Request(f"{SITE}/wp-json/wp/v2/posts?per_page=100&_fields=id,title,content", headers=HEADERS)
for p in json.loads(urllib.request.urlopen(req).read()):
    if '.arcaea-article-content' in p['content']['rendered']:
        continue  # 已统一
    new_content = transform_content(p['content']['rendered'])
    data = json.dumps({"content": new_content}).encode()
    req2 = urllib.request.Request(f"{SITE}/wp-json/wp/v2/posts/{p['id']}", data=data, headers=HEADERS, method="POST")
    urllib.request.urlopen(req2, timeout=30)
    print(f"Updated: ID {p['id']}")
```
