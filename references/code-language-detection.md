# 代码语言自动标注参考

用于批量给 WordPress 文章中 `<pre><code>` 块添加 `class="language-xxx"` 的 Python 语言检测函数。

## 核心检测逻辑

```python
def detect_language(code_text):
    """Detect programming language from code block content"""
    text = code_text.strip()
    first_line = text.split('\n')[0].strip() if text else ''
    
    # === Mermaid (skip, handled by babel-arcaea-code plugin) ===
    if any(kw in text for kw in ['flowchart', 'sequenceDiagram', 'stateDiagram', 
                                  'classDiagram', 'gantt', 'pie ', 'mindmap', 'gitGraph']):
        return 'mermaid'
    
    # === JSON: starts with { and has "key": value pattern ===
    if first_line.startswith('{') or text.startswith('{'):
        if re.search(r'"[^"]+":\s*"', text) or re.search(r'"[^"]+":\s*\[', text):
            return 'json'
        if text.count('":') >= 2 and text.count('{') >= 1:
            return 'json'
    
    # === YAML: line starts with "key:" ===
    if re.match(r'^[a-zA-Z_][a-zA-Z0-9_]*:', first_line) and '://' not in first_line:
        if not text.startswith('#include') and ';' not in first_line:
            return 'yaml'
    
    # === Dart/Flutter ===
    if re.search(r"import\s+'package:", text) or \
       (re.search(r'\bclass\s+\w+\s*(?:extends|implements|{)', text) and 'public:' not in text):
        if not any(cpp in text for cpp in ['#include', 'HAL_', 'STM32', 
                   'UART_HandleTypeDef', 'GPIO_InitTypeDef']):
            return 'dart'
    
    # === C/C++ (embedded STM32 HAL, FreeRTOS) ===
    c_kw = ['#include', '#define', '#ifndef', '#pragma', 'int main(', 
            'HAL_', 'STM32', 'HandleTypeDef', 'osThreadId', 'xTaskCreate', 
            'configASSERT', 'HAL_StatusTypeDef', 'void HAL_', 'void MX_', 
            'typedef enum', 'typedef struct']
    if any(kw in text for kw in c_kw):
        return 'c'
    if re.search(r'\b(void|static|uint8_t|uint16_t|uint32_t|int8_t|int16_t)\b', text) \
       and re.search(r'\(.*?\)\s*\{', text):
        return 'c'
    
    # === Bash/shell ===
    bash_cmd = r'^(sudo|apt|pacman|pip|export |cd |mkdir|git |curl |wget|' \
               r'make|winget|usbip|dism|cmake |echo |cat |rm |cp |mv |ls |' \
               r'chmod |chown |source )'
    if first_line.startswith('$ ') or first_line.startswith('#!') \
       or re.search(bash_cmd, first_line):
        return 'bash'
    # Also check first non-comment line
    lines = [l.strip() for l in text.split('\n') 
             if l.strip() and not l.strip().startswith('#')]
    if lines and re.search(bash_cmd, lines[0]):
        return 'bash'
    
    # === CMake DSL (not shell command) ===
    if any(kw in text for kw in ['cmake_minimum_required', 'add_executable', 
            'target_link_libraries', 'project(', 'set(CMAKE', 'find_package(', 
            'add_subdirectory', 'set(CMAKE_SYSTEM']):
        return 'cmake'
    
    # === Python ===
    if re.search(r'\bdef\s+\w+\s*\(', text) or re.search(r'\bclass\s+\w+:', text):
        if '#include' not in text:
            return 'python'
    
    return 'text'
```

## 批量处理

```python
import re, base64, json, urllib.request, time

site = "https://babel36acl.xyz"
auth = base64.b64encode(b"username:app-password").decode()
headers = {"Authorization": f"Basic {auth}", "Content-Type": "application/json"}

# 1. Fetch all posts
req = urllib.request.Request(
    f"{site}/wp-json/wp/v2/posts?per_page=100&context=edit&_fields=id,title,content",
    headers=headers
)
resp = urllib.request.urlopen(req)
posts = json.loads(resp.read().decode())

# 2. Fix function: add language class + wrap bare <pre>
def fix_blocks(content):
    # Step A: <pre><code class="language-text"> → correct language
    pat = r'(<pre[^>]*>)\s*(<code[^>]*class="language-([^"]*)"[^>]*>)(.*?)(</code>\s*</pre>)'
    def upgrade(m):
        pre, code, cur, txt, close = m.groups()
        if cur != 'text':
            return m.group(0)
        detected = detect_language(txt)
        if detected in ('text', 'mermaid'):
            return m.group(0)
        new_code = code.replace('class="language-text"', f'class="language-{detected}"')
        return pre + new_code + txt + close
    content = re.sub(pat, upgrade, content, flags=re.DOTALL)
    
    # Step B: <pre><code> with no class → add detected language
    pat2 = r'(<pre[^>]*>)\s*(<code[^>]*>)(.*?)(</code>\s*</pre>)'
    def add_lang(m):
        pre, code, txt, close = m.groups()
        if 'language-' in code or 'class="mermaid"' in pre:
            return m.group(0)
        lang = detect_language(txt)
        new_code = code.replace('<code', f'<code class="language-{lang}"')
        return pre + new_code + txt + close
    content = re.sub(pat2, add_lang, content, flags=re.DOTALL)
    
    # Step C: Bare <pre> → wrap in <code>
    def wrap_bare(m):
        pre_tag = m.group(1)
        inner = m.group(2)
        if '<code' in inner or 'class="mermaid"' in m.group(0):
            return m.group(0)
        inner = inner.strip()
        lang = detect_language(inner)
        return f'<{pre_tag}><code class="language-{lang}">{inner}</code></pre>'
    
    content = re.sub(r'<(pre[^>]*)>(.*?)</pre>', wrap_bare, content, flags=re.DOTALL)
    return content

# 3. Update each post
for p in posts:
    content = p['content']['raw']
    fixed = fix_blocks(content)
    if fixed == content:
        continue
    data = json.dumps({"content": fixed}).encode()
    req = urllib.request.Request(
        f"{site}/wp-json/wp/v2/posts/{p['id']}",
        data=data, headers=headers, method="POST"
    )
    resp = urllib.request.urlopen(req)
    time.sleep(0.3)
```

## 已处理的语言类型

| `language-xxx` | 检测条件 |
|----------------|---------|
| `c` | `#include`, `HAL_`, `STM32`, `HandleTypeDef`, `void function()` |
| `dart` | `import 'package:`, `class extends`, `Future<` |
| `bash` | `$ `, `sudo`, `apt`, `cmake --`, `git`, `pacman`, `echo`, `cat` |
| `cmake` | `cmake_minimum_required`, `target_link_libraries`, `project()` |
| `json` | 以 `{` 开头，含 `"key":` 模式（不依赖结尾 `}`，编辑器可能截断） |
| `yaml` | 行首 `key:` 模式，不包含 C 代码特征 |
| `python` | `def function(`, `class Name:` |
| `text` | 文件树、注释、无法识别的纯文本 |

## 常见问题

1. **JSON 块可能被截断**（WordPress 编辑器不保存完整 JSON）→ 不能依赖 `'}' in text` 检查。改为检查 `"key":` 模式
2. **`cmake` 命令 vs `cmake` DSL**：命令行 `cmake --preset Debug` → `bash`；CMake 语法 `cmake_minimum_required(...)` → `cmake`
3. **行内 `<code>` 不处理**：仅处理 `<pre><code>` 完整代码块，行内 `<code>` 引用（函数名、变量名）不加语言类
4. **`context=edit`**：始终使用 `?context=edit` 获取 `content.raw`，避免 wpautop HTML 污染干扰 regex 匹配
