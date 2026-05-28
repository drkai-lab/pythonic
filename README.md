# pythonic

(日本語はこちら → README.ja.md | 简体中文 → README.zh-CN.md | 繁體中文 → README.zh-TW.md | Español → README.es.md | Русский → README.ru.md)

Forces agents to write the Python that senior engineers actually ship in 2026. Targets 3.10+, leans hard into 3.12+ stdlib. No 2018-era sludge from training data.

## Install

The skill is a single file. The directory it lives in must be named `pythonic` (matches the `name:` in the frontmatter).

**User-wide** (recommended):

Unix/macOS:

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

**Inside a project** (committed with the repo):

```bash
mkdir -p .grok/skills/pythonic
cp skill.md .grok/skills/pythonic/
```

Windows equivalent (PowerShell):

```powershell
$target = Join-Path ".grok" "skills/pythonic"
New-Item -ItemType Directory -Force -Path $target | Out-Null
Copy-Item skill.md -Destination (Join-Path $target "skill.md")
```

Clone once:

```bash
git clone https://github.com/drkai-lab/pythonic.git
cd pythonic
# then one of the copy commands above
```

Claude Code users can drop the same file into `.claude/skills/pythonic/` (rename to SKILL.md if your harness is strict about the filename). Cursor users can lift the core rules into `.cursor/rules` or `.mdc` files with minimal editing.

## Two concrete examples

**File handling (the os.path habit that refuses to die):**

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

**Type hints (still the #1 thing agents get wrong in 2026):**

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

The full set of rules, the large anti-pattern table, version feature gates, and strict enforcement list live in [skill.md](skill.md). Read that file.

## Why this exists

Left to their own devices, agents emit `from typing import List`, `os.path.join`, bare `except:`, seven-deep if/elif type ladders, and `class Foo(object):` because that's what dominated the internet for fifteen years.

This skill is the standing correction. It bakes in the current consensus from the Python core team, typing-sig, and what survives real review at shops that treat the language seriously.

## Contributing

Changes are welcome only if you can explain why a senior Pythonista who has shipped production code in the last two years would prefer the new version. Start by reading the top of skill.md.

## License

MIT. See LICENSE.
