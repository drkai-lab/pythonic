# pythonic

(English → README.md | 简体中文 → README.zh-CN.md | 繁體中文 → README.zh-TW.md | Español → README.es.md | Русский → README.ru.md)

エージェントに、2026年現在で本番投入できるレベルのPythonを書かせるためのスキルです。3.10以上を対象に、3.12以降の標準ライブラリ機能を積極的に使います。学習データ由来の古い書き方は一切許しません。

## インストール

スキル本体は1ファイルです。配置先のディレクトリ名は必ず `pythonic` にしてください（frontmatterの `name:` と一致させるため）。

**ユーザー全体で使う場合**（おすすめ）:

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

**プロジェクト内だけで使う場合**（リポジトリにコミット）:

```bash
mkdir -p .grok/skills/pythonic
cp skill.md .grok/skills/pythonic/
```

Windows PowerShellの場合:

```powershell
$target = Join-Path ".grok" "skills/pythonic"
New-Item -ItemType Directory -Force -Path $target | Out-Null
Copy-Item skill.md -Destination (Join-Path $target "skill.md")
```

最初にクローン:

```bash
git clone https://github.com/drkai-lab/pythonic.git
cd pythonic
# 上記のいずれかのコピーコマンドを実行
```

## 他のエージェントでも使えます

このスキルは2026年現在で複数のエージェントツールが共通で理解できる `SKILL.md` 形式を採用しています。

**対応している配置先**（ディレクトリ名は必ず `pythonic` にしてください）:

- Grok: `~/.grok/skills/pythonic/skill.md`
- Opencode: `~/.config/opencode/skills/pythonic/SKILL.md`（プロジェクト内は `.opencode/skills/pythonic/SKILL.md` も可）
- Claude Code: `~/.claude/skills/pythonic/SKILL.md`（ファイル名を `SKILL.md` に変更する必要があります）
- HermesAgent: `~/.hermes/skills/pythonic/SKILL.md`
- Cursor / Codex系: `.cursor/rules/` に `.mdc` として置くか、`.claude/skills/` のパスを流用

追加後はTUIやエージェントの再起動、またはスキルリロードコマンドを実行してください。

UnixでのOpencode向けの例:

```bash
git clone https://github.com/drkai-lab/pythonic.git /tmp/p && \
mkdir -p ~/.config/opencode/skills/pythonic && \
cp /tmp/p/skill.md ~/.config/opencode/skills/pythonic/SKILL.md
```

Windows (PowerShell) の場合は、Grokの項目で示した `Join-Path` + `New-Item -Force` のパターンで同じディレクトリ構造に置けば動作します。

## 具体例（2つ）

**ファイル操作（いまだに根絶しないos.path病）:**

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

**型ヒント（2026年でもエージェントが一番間違えるところ）:**

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

ルールの全容、アンチパターンの大表、バージョンごとの利用可能機能、厳格な強制ルールはすべて [skill.md](skill.md) に書いてあります。必ず読んでください。

## なぜ作ったのか

エージェントは放っておくと `from typing import List`、`os.path.join`、裸の `except:`、7段にもなるif/elifの型チェック、`class Foo(object):` などを平気で出してきます。インターネットに15年分積もった古いPythonコードが学習データの大部分を占めているからです。

このスキルはその是正装置です。Pythonコアチームやtyping-sigが現在推奨している書き方と、実際に本番でコードレビューを通る水準を固めてあります。

## コントリビュート

変更を提案する場合は、「直近2年以内に本番Pythonコードを出荷したベテラン開発者が、なぜこちらを好むのか」を説明できるものに限ります。まずはskill.mdの冒頭を読んでからにしてください。

## ライセンス

MIT。LICENSE を参照してください。
