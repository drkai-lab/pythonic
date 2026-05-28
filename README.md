# pythonic

(日本語はこちら → README.ja.md | 简体中文 → README.zh-CN.md | 繁體中文 → README.zh-TW.md | Español → README.es.md | Русский → README.ru.md)

A Grok skill that makes it write the Python a senior dev would actually ship, not the usual model output.

## Install

**Personal** (loads for every conversation):

```powershell
git clone https://github.com/drkai-lab/pythonic.git "$env:USERPROFILE\.grok\skills\pythonic"
```

**Inside a project** (for a specific repo or team):

```powershell
# run from your repository root
git clone https://github.com/drkai-lab/pythonic.git .\.grok\skills\pythonic
```

No special tooling required. Plain clone works. Reload skills in Grok after adding.

For Claude Code, Cursor, Windsurf, or anything else: `skill.md` is plain text. You can paste the anti-patterns table, turn it into project rules, or convert the whole thing to .mdc format. The value is in the concrete replacements, not the Grok wrapper.

## What actually changes

Modern type hints instead of 2018-era imports:

```python
# what most models still emit
from typing import List, Optional, Dict
def process(items: List[str]) -> Optional[Dict[str, int]]: ...

# what this skill produces
def process(items: list[str]) -> dict[str, int] | None: ...
```

Path handling and iteration that doesn't make you wince:

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

See [skill.md](skill.md) for the complete set: structural pattern matching vs if-elif chains, when `dataclass(frozen=True, slots=True)` beats everything, `asyncio.TaskGroup`, `zoneinfo`, the 25+ row anti-patterns table, version gates, and the ten hard enforcement rules.

## What this skill is

It targets Python 3.10+ and defaults to 3.12+ idioms wherever they exist. Every public function and module-level name gets a real annotation. Stdlib over third-party when the stdlib finally caught up. pytest, not unittest. No bare `except:`, no mutable defaults, no `# type: ignore` without a story.

The goal is simple: stop shipping code that looks like an LLM was trained on old Stack Overflow answers.

If you're a working Python developer who has ever sighed at generated code using `os.path.join` and `typing.List`, this is the narrow corrective.

Issues and PRs that document newly common patterns in real codebases are welcome. The rest is already written down.

## License

MIT. See [LICENSE](LICENSE).
