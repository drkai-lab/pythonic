# pythonic

(English → README.md | 日本語 → README.ja.md | 简体中文 → README.zh-CN.md | Español → README.es.md | Русский → README.ru.md)

讓 Grok 寫出資深 Python 開發者真正會提交的程式碼，而不是一般大模型的輸出。

## 安裝

**個人使用**（所有對話都生效）:

```powershell
git clone https://github.com/drkai-lab/pythonic.git "$env:USERPROFILE\.grok\skills\pythonic"
```

**專案內使用**（針對特定儲存庫或團隊）:

```powershell
# 在你的儲存庫根目錄執行
git clone https://github.com/drkai-lab/pythonic.git .\.grok\skills\pythonic
```

不需要特別的工具。直接 git clone 就好。加入後記得在 Grok 重新載入技能。

如果是 Claude Code、Cursor、Windsurf 等其他工具：`skill.md` 就是純文字檔。你可以把反模式表複製到專案規則、轉成 .mdc 格式，或直接餵給模型用。真正實用的地方在那些具體的替代寫法。

## 實際會改變什麼

現代的型別提示，而不是 2018 年的老寫法：

```python
# 大多數模型現在還是會輸出的
from typing import List, Optional, Dict
def process(items: List[str]) -> Optional[Dict[str, int]]: ...

# 這個技能會寫成這樣
def process(items: list[str]) -> dict[str, int] | None: ...
```

不會讓人皺眉的路徑處理與迭代：

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

完整內容（結構化模式匹配跟 if-elif 該怎麼選、`dataclass(frozen=True, slots=True)` 什麼時候該用、`asyncio.TaskGroup`、`zoneinfo`、超過 25 行的反模式表、各版本功能對照，以及 10 條硬性規則）全部寫在 [skill.md](skill.md) 裡。

## 這個技能到底在做什麼

針對 Python 3.10 以上，能用 3.12+ 的新寫法就優先採用。所有公開函式和模組層級的名稱都必須要有真正的型別註解。標準庫能做到的事就不拉第三方套件。測試用 pytest 風格，絕對不碰 unittest.TestCase。裸 `except:`、可變預設參數、沒理由的 `# type: ignore` 一律禁止。

目的很簡單：不要再交出那種一看就知道是舊 Stack Overflow 教學或偷懶模型生出來的程式碼。

如果你是實際在寫 Python 的開發者，曾經因為 AI 給你一堆 `os.path.join` 和 `typing.List` 而嘆氣，這個技能就是用來治這個毛病的。

如果你在真實專案裡看到已經被廣泛採用的新習慣，歡迎開 issue 或送 PR。其餘內容基本上都已經寫在 skill.md 裡了。

## 授權

MIT。詳見 [LICENSE](LICENSE)。
