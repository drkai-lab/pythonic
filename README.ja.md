(English → README.md | 简体中文 → README.zh-CN.md | 繁體中文 → README.zh-TW.md | Español → README.es.md | Русский → README.ru.md)

# pythonic

Python 3.10+（3.12+ 推奨）向けの、シニアエンジニアが本気でレビューを通すレベルの厳格なガイドライン。型安全で stdlib を優先し、「AI が書いた感」のコードを徹底的に排除する。

## インストール

**個人用**（マシン全体で有効）:

PowerShell:
```powershell
git clone https://github.com/drkai-lab/pythonic $env:USERPROFILE\.grok\skills\pythonic
```

bash / zsh:
```bash
git clone https://github.com/drkai-lab/pythonic ~/.grok/skills/pythonic
```

**プロジェクト用**（リポジトリにコミットしてチームで共有）:

```powershell
# PowerShell（&& 禁止）
mkdir -p .grok\skills
git clone --depth 1 https://github.com/drkai-lab/pythonic .grok\skills\pythonic
```

Unix 版はパス区切りを `/` に置き換えるだけ。

手動クローンでももちろん動く。他のツール（Claude Code、Cursor など）を使う場合は `skill.md` をそのまま読み込んで、ルール部分を各自のフォーマットに変換して使えばいい。

## 実際の差（1例）

よくある生成コード:
```python
def process(data):
    result = []
    for item in data:
        if item > 0:
            result.append(item * 2)
    return result
```

Pythonic 版:
```python
def process(items: list[float]) -> list[float]:
    return [x * 2 for x in items if x > 0]
```

他にも 30 以上の具体例が [skill.md](skill.md) のアンチパターンテーブルに載っている。

## 実際に守らせるルール（抜粋）

- すべての関数・メソッドに返り値の型アノテーションを必須とする
- ファイル操作は `pathlib.Path` 一択。`os.path` は禁止
- f-string 以外認めない。`%` も `.format()` も使わせない
- 型や構造による分岐は `match` を使い、長大な `isinstance` チェーンは臭いと判断
- 裸の `except:`、ミュータブルなデフォルト引数、理由のない `# type: ignore` は全部ダメ

ターゲットの Python バージョンが不明な場合は 3.12+ の記法をデフォルトにし、必要ならバージョンを聞く。

完全版（17 セクション + バージョン対応表 + 大アンチパターンテーブル）は [skill.md](skill.md) を見ること。

## なぜ作ったか

LLM が出す Python は一見それっぽく見えるが、半年後には「これ誰が書いたんだ…」となることが多い。このスキルは実際に痛い目を見てきた人たちが積み上げた「これだけは守れ」というリストだ。

意図的に短くて尖っている。ふわっとした「ベストプラクティス」が欲しいなら、別のスキルを使ってほしい。

## 貢献

3.12+ の新しいイディオムや、より鋭い反例を見つけたら PR を。例は短く、現実的で、同じトーンのままに。ドキュメントにも「AIスロップ禁止」の基準を適用する。

## ライセンス

MIT — [LICENSE](LICENSE) を参照。
