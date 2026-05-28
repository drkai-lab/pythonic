# pythonic

(English → README.md | 日本語はこちら → README.ja.md | 繁體中文 → README.zh-TW.md | Español → README.es.md | Русский → README.ru.md)

让智能体写出 2026 年真正能上生产环境的 Python。目标版本 3.10+，优先使用 3.12+ 的标准库特性。训练数据里那些 2018 年的老代码写法一概不允许出现。

## 安装

技能只有一个文件。放置目录必须命名为 `pythonic`（要和 frontmatter 里的 `name:` 一致）。

**用户级**（推荐，机器上到处可用）:

Unix / macOS:

```bash
mkdir -p ~/.grok/skills/pythonic
cp skill.md ~/.grok/skills/pythonic/
```

Windows PowerShell:

```powershell
$target = Join-Path $HOME ".grok/skills/pythonic"
New-Item -ItemType Directory -Force -Path $target | Out-Null
Copy-Item skill.md -Destination (Join-Path $target "skill.md")
```

**项目级**（和仓库一起提交）:

```bash
mkdir -p .grok/skills/pythonic
cp skill.md .grok/skills/pythonic/
```

Windows PowerShell 写法:

```powershell
$target = Join-Path ".grok" "skills/pythonic"
New-Item -ItemType Directory -Force -Path $target | Out-Null
Copy-Item skill.md -Destination (Join-Path $target "skill.md")
```

先克隆仓库:

```bash
git clone https://github.com/drkai-lab/pythonic.git
cd pythonic
# 然后执行上面任意一条复制命令
```

Claude Code 用户可以把同一个文件放到 `.claude/skills/pythonic/` 下（如果你的环境对文件名有要求，就改成 SKILL.md）。Cursor 用户可以把核心规则手动提取到 `.cursor/rules` 或 `.mdc` 文件里，改动量很小。

## 两个真实例子

**文件路径处理（怎么也死不掉的 os.path 习惯）：**

```python
# Before
import os
def load(path):
    full = os.path.join(path, "config.toml")
    with open(full) as f:
        ...
```

```python
# After
from pathlib import Path
import tomllib

def load(base: str | Path) -> dict:
    p = Path(base) / "config.toml"
    return tomllib.loads(p.read_text(encoding="utf-8"))
```

**类型提示（2026 年智能体仍然最容易搞砸的地方）：**

```python
# Before
from typing import List, Dict, Optional, Union

def process(items: List[Dict[str, Union[str, int]]]) -> Optional[Dict]:
    ...
```

```python
# After
def process(items: list[dict[str, str | int]]) -> dict | None:
    ...
```

完整规则、反模式对照大表、各版本特性开关、强制执行清单都在 [skill.md](skill.md) 里。一定要读完那一份。

## 为什么要做这个

智能体如果不加约束，就会吐出 `from typing import List`、`os.path.join`、裸 `except:`、七层 if/elif 类型判断、`class Foo(object):` 之类的代码。因为过去十五年互联网上堆积的 Python 旧教程和旧答案占了训练数据的绝大部分。

这个技能就是专门用来纠正这一点的。它把 Python 核心团队、typing-sig 当前的共识，以及真正能通过生产代码审查的写法固化下来了。

## 贡献

只有当你能说清楚「过去两年内真正往生产环境发过 Python 代码的资深开发者，为什么会更喜欢新写法」时，我们才欢迎改动。建议先读 skill.md 开头部分。

## 许可证

MIT。详见 LICENSE。
