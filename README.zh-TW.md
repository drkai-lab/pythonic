# pythonic

(English → README.md | 日本語はこちら → README.ja.md | 简体中文 → README.zh-CN.md | Español → README.es.md | Русский → README.ru.md)

強制讓智能體寫出 2026 年真正能放到生產環境的 Python。目標版本 3.10+，優先採用 3.12+ 標準函式庫的現代寫法。訓練資料裡那些 2018 年的老舊模式一律不准出現。

## 安裝

技能只有一個檔案。放置的資料夾名稱必須是 `pythonic`（要和 frontmatter 中的 `name:` 一致）。

**使用者層級**（推薦，電腦上隨處可用）:

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

**專案層級**（跟隨倉庫一起提交）:

```bash
mkdir -p .grok/skills/pythonic
cp skill.md .grok/skills/pythonic/
```

Windows PowerShell 寫法:

```powershell
$target = Join-Path ".grok" "skills/pythonic"
New-Item -ItemType Directory -Force -Path $target | Out-Null
Copy-Item skill.md -Destination (Join-Path $target "skill.md")
```

先複製倉庫:

```bash
git clone https://github.com/drkai-lab/pythonic.git
cd pythonic
# 再執行上面任一複製指令
```

## 也支援其他 Agent

這個 skill 採用 2026 年多款 agent 工具共通的 `SKILL.md` 格式。

**支援的放置位置**（資料夾名稱必須是 `pythonic`）：

- Grok: `~/.grok/skills/pythonic/skill.md`
- Opencode: `~/.config/opencode/skills/pythonic/SKILL.md`（專案內可用 `.opencode/skills/pythonic/SKILL.md`）
- Claude Code: `~/.claude/skills/pythonic/SKILL.md`（需要將檔名改為 `SKILL.md`）
- HermesAgent: `~/.hermes/skills/pythonic/SKILL.md`
- Cursor / Codex 系: 放到 `.cursor/rules/` 做成 `.mdc`，或直接沿用 `.claude/skills/` 路徑

放置完成後請重啟 TUI / Agent，或執行對應的 skill reload 指令。

Unix 給 Opencode 的快速複製範例：

```bash
git clone https://github.com/drkai-lab/pythonic.git /tmp/p && \
mkdir -p ~/.config/opencode/skills/pythonic && \
cp /tmp/p/skill.md ~/.config/opencode/skills/pythonic/SKILL.md
```

Windows PowerShell 使用者請參考前面 Grok 章節的 `Join-Path` + `New-Item -Force` 模式建立相同目錄結構。

## 兩個具體例子

**檔案路徑處理（怎麼樣都改不掉的 os.path 壞習慣）：**

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

**型別提示（2026 年智能體依然最常寫錯的地方）：**

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

完整規則、反模式對照表、各版本功能開關、強制執行清單全部寫在 [skill.md](skill.md)。務必讀過那一份。

## 為什麼要做這個

智能體如果沒有約束，就會直接吐出 `from typing import List`、`os.path.join`、裸 `except:`、七層 if/elif 型別判斷、`class Foo(object):` 之類的東西。因為過去十五年網路上堆積的舊 Python 教學和舊問答占了訓練資料的絕大部分。

這個技能就是用來糾正這件事的。它把 Python 核心團隊與 typing-sig 現在的共識，以及真正能通過生產環境程式碼審查的寫法固化下來。

## 貢獻

只有當你能清楚說明「過去兩年內真正把 Python 程式碼發佈到生產環境的資深開發者，為什麼會偏好新寫法」時，我們才接受修改建議。請先閱讀 skill.md 的開頭部分。

## 授權

MIT。詳情請見 LICENSE。
