# pythonic

(English → README.md | 日本語 → README.ja.md | 繁體中文 → README.zh-TW.md | Español → README.es.md | Русский → README.ru.md)

让 Grok 写出资深 Python 开发者真正会交的代码，而不是常见的大模型输出。

## 安装

**个人使用**（所有对话中都生效）:

```powershell
git clone https://github.com/drkai-lab/pythonic.git "$env:USERPROFILE\.grok\skills\pythonic"
```

**项目内使用**（针对某个仓库或团队）:

```powershell
# 在你的仓库根目录执行
git clone https://github.com/drkai-lab/pythonic.git .\.grok\skills\pythonic
```

不需要任何特殊工具。直接 git clone 就行。加完之后在 Grok 里重新加载技能即可。

对于 Claude Code、Cursor、Windsurf 等其他工具：`skill.md` 就是纯文本。你可以把反模式表贴到项目规则里，转成 .mdc，或者直接喂给模型。真正有价值的是那些具体的替换例子。

## 实际会变什么

现代类型提示，而不是 2018 年的老写法：

```python
# 大多数模型现在还爱输出的
from typing import List, Optional, Dict
def process(items: List[str]) -> Optional[Dict[str, int]]: ...

# 这个技能会写成这样
def process(items: list[str]) -> dict[str, int] | None: ...
```

不会让人皱眉的路径和迭代写法：

```python
# before
import os
for f in os.listdir("data"):
    if f.endswith(".txt"):
        ...

# after
from pathlib import Path
for p in Path("data").glob("*.txt"):
    ...
```

完整内容（结构化模式匹配跟 if-elif 链怎么选、`dataclass(frozen=True, slots=True)` 什么时候该用、`asyncio.TaskGroup`、`zoneinfo`、25 行以上的反模式表、版本特性对照表，以及 10 条硬性规则）全在 [skill.md](skill.md) 里。

## 这个技能到底在干什么

针对 Python 3.10+，能用 3.12+ 的新写法就优先用。所有公开函数和模块级名字都必须有真正的类型注解。标准库能干的事就不拉第三方库。测试用 pytest 风格，坚决不用 unittest.TestCase。裸 `except:`、可变默认参数、没来由的 `# type: ignore` 一律不许出现。

目的就一个：别再往外扔那种一看就是老 Stack Overflow 教程或者偷懒模型生成的代码。

如果你平时真在写 Python，曾经看到 AI 甩给你一堆 `os.path.join` 和 `typing.List` 而叹过气，这个技能就是专门治这个的。

如果你发现真实项目里已经普遍采用的新习惯，欢迎提 issue 或 PR。其他内容基本都写在 skill.md 里了。

## 许可证

MIT。见 [LICENSE](LICENSE)。
