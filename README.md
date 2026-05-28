(日本語はこちら → README.ja.md | 简体中文 → README.zh-CN.md | 繁體中文 → README.zh-TW.md | Español → README.es.md | Русский → README.ru.md)

# pythonic

Strict, typed, stdlib-first Python for 3.10+ (prefer 3.12+). The kind a senior dev would actually accept in review. Designed to kill the vague, over-engineered, "AI wrote this" code that doesn't age well.

## Install

**Personal** (your machine, all projects):

PowerShell:
```powershell
git clone https://github.com/drkai-lab/pythonic $env:USERPROFILE\.grok\skills\pythonic
```

bash / zsh:
```bash
git clone https://github.com/drkai-lab/pythonic ~/.grok/skills/pythonic
```

**Project** (committed so the whole team follows the same rules):

```powershell
# PowerShell — separate commands, no &&
mkdir -p .grok\skills
git clone --depth 1 https://github.com/drkai-lab/pythonic .grok\skills\pythonic
```

Unix version is the obvious translation with `/`.

Manual clone works too. Other tools (Claude Code, Cursor rules, etc.): `skill.md` is plain text. Copy or lightly adapt the enforcement rules and anti-pattern table yourself.

## One concrete before/after

Common generated pattern:
```python
def process(data):
    result = []
    for item in data:
        if item > 0:
            result.append(item * 2)
    return result
```

Pythonic:
```python
def process(items: list[float]) -> list[float]:
    return [x * 2 for x in items if x > 0]
```

See 30+ more in the anti-pattern table inside [skill.md](skill.md).

## What the rules actually enforce (excerpt)

- All functions and methods **must** have return type annotations.
- `pathlib` everywhere. `os.path.join` and manual string paths are gone.
- f-strings only. No `%` formatting, no `.format()`.
- `match` for dispatching on type or structure. Long `isinstance` chains are a smell.
- No bare `except:`, no mutable defaults, no unexplained `# type: ignore`.
- Default to 3.12+ features unless you tell it the target version.

The complete list of 17 sections, version feature gates, and the big anti-pattern table lives in [skill.md](skill.md).

## Why this exists

Most Python that comes out of LLMs looks reasonable until you have to live with it for a year. This skill is the distilled set of things that have actually caused pain in real codebases.

It is deliberately terse and opinionated. No marketing. No 12-step onboarding. If you want gentle suggestions, use a different skill.

## Contributing

PRs that add real 3.12+ patterns or sharper counter-examples are welcome. Keep examples short, real, and in the same direct tone. The "no AI slop" bar applies to the docs too.

## License

MIT — see [LICENSE](LICENSE).
