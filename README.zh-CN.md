(English → README.md | 日本語 → README.ja.md | 繁體中文 → README.zh-TW.md | Español → README.es.md | Русский → README.ru.md)

# pythonic

针对 Python 3.10+（优先 3.12+）的严格指南。强调类型安全、标准库优先，写出资深开发者真正能接受的代码。专门用来杜绝那种“看起来像 AI 写的”、短期能跑但长期会出问题的代码。

## 安装

**个人使用**（全局生效）:

PowerShell:
```powershell
git clone https://github.com/drkai-lab/pythonic $env:USERPROFILE\.grok\skills\pythonic
```

bash / zsh:
```bash
git clone https://github.com/drkai-lab/pythonic ~/.grok/skills/pythonic
```

**项目内使用**（提交到仓库，全团队统一）:

```powershell
# PowerShell（不要用 && 串联）
mkdir -p .grok\skills
git clone --depth 1 https://github.com/drkai-lab/pythonic .grok\skills\pythonic
```

Unix 系统把路径分隔符换成 `/` 即可。

手动 clone 也完全没问题。其他工具（Claude Code、Cursor 等）：核心规则都在 `skill.md` 里，直接拿去转成对应格式就行。

## 一个真实的对比

常见 AI 生成写法:
```python
def process(data):
    result = []
    for item in data:
        if item > 0:
            result.append(item * 2)
    return result
```

符合 Pythonic 的写法:
```python
def process(items: list[float]) -> list[float]:
    return [x * 2 for x in items if x > 0]
```

更多 30 多条真实反例都在 [skill.md](skill.md) 的反模式表格里。

## 强制执行的规则（部分）

- 所有函数和方法都必须写返回类型注解
- 文件路径操作一律用 `pathlib`，`os.path` 直接禁掉
- 只允许 f-string，`%` 和 `.format()` 都不行
- 类型或结构分发用 `match`，一长串 `isinstance` 判断就是坏味道
- 禁止裸 `except:`、可变默认参数、没理由的 `# type: ignore`

不知道目标 Python 版本时，默认按 3.12+ 的新写法来，需要时会主动问版本。

完整内容（17 个章节 + 特性版本对照表 + 大量反模式）都在 [skill.md](skill.md)。

## 为什么要做这个

LLM 吐出来的 Python 当时看着挺顺眼，半年后维护的时候经常后悔。这套规则是真实代码库里踩过坑的人总结出来的「必须死守」的清单。

写得故意很直接、很较真。不喜欢这种风格的，可以换别的 skill。

## 贡献

如果你有 3.12+ 的新用法，或者找到更好的反例，欢迎提 PR。例子要短、要真实，语气保持一致。文档本身也必须过「没有 AI 味」的关。

## 许可证

MIT — 详见 [LICENSE](LICENSE)。
