(English → README.md | 日本語 → README.ja.md | 简体中文 → README.zh-CN.md | Español → README.es.md | Русский → README.ru.md)

# pythonic

針對 Python 3.10+（優先 3.12+）的嚴格指南。強調型別安全、以標準函式庫為主，寫出資深開發者真正願意接受的程式碼。專門用來杜絕那種「看起來像 AI 寫的」、短期能跑但長期會出問題的程式碼。

## 安裝

**個人使用**（全域生效）:

PowerShell:
```powershell
git clone https://github.com/drkai-lab/pythonic $env:USERPROFILE\.grok\skills\pythonic
```

bash / zsh:
```bash
git clone https://github.com/drkai-lab/pythonic ~/.grok/skills/pythonic
```

**專案內使用**（提交到 repo，全團隊統一）:

```powershell
# PowerShell（不要用 && 串接）
mkdir -p .grok\skills
git clone --depth 1 https://github.com/drkai-lab/pythonic .grok\skills\pythonic
```

Unix 系統只要把路徑分隔符換成 `/` 即可。

手動 clone 也完全沒問題。其他工具（Claude Code、Cursor 等）：核心規則都在 `skill.md`，直接拿去轉成對應格式就行。

## 一個真實的對比

常見 AI 生成寫法:
```python
def process(data):
    result = []
    for item in data:
        if item > 0:
            result.append(item * 2)
    return result
```

符合 Pythonic 的寫法:
```python
def process(items: list[float]) -> list[float]:
    return [x * 2 for x in items if x > 0]
```

更多 30 多條真實反例都在 [skill.md](skill.md) 的反模式表格裡。

## 強制執行的規則（部分）

- 所有函式與方法都必須寫回傳型別註解
- 檔案路徑操作一律使用 `pathlib`，`os.path` 直接禁用
- 只允許 f-string，`%` 與 `.format()` 都不行
- 型別或結構分派使用 `match`，一長串 `isinstance` 判斷就是壞味道
- 禁止裸 `except:`、可變預設參數、沒有理由的 `# type: ignore`

不知道目標 Python 版本時，預設採用 3.12+ 的新寫法，需要時會主動詢問版本。

完整內容（17 個章節 + 特性版本對照表 + 大量反模式）都在 [skill.md](skill.md)。

## 為什麼要做這個

LLM 吐出來的 Python 當時看起來挺順眼，半年後維護的時候經常後悔。這套規則是真實程式碼庫裡踩過坑的人總結出來的「必須死守」的清單。

寫得故意很直接、很較真。不喜歡這種風格的，可以換別的 skill。

## 貢獻

如果你有 3.12+ 的新用法，或者找到更好的反例，歡迎提 PR。例子要短、要真實，語氣保持一致。文件本身也必須通過「沒有 AI 味」的檢驗。

## 授權

MIT — 詳見 [LICENSE](LICENSE)。
