# pythonic

(English → README.md | 简体中文 → README.zh-CN.md | 繁體中文 → README.zh-TW.md | Español → README.es.md | Русский → README.ru.md)

Grokに、本物のシニアPython開発者が書くようなコードを書かせるためのスキルです。よくあるモデルの出力とは違います。

## インストール

**個人用**（すべての会話で読み込まれる）:

```powershell
git clone https://github.com/drkai-lab/pythonic.git "$env:USERPROFILE\.grok\skills\pythonic"
```

**プロジェクト内用**（特定のレポジトリやチームで使う場合）:

```powershell
# レポジトリのルートディレクトリで実行
git clone https://github.com/drkai-lab/pythonic.git .\.grok\skills\pythonic
```

特別なツールは必要ありません。普通にgit cloneするだけで動きます。追加したらGrok側でスキルをリロードしてください。

Claude Code、Cursor、Windsurfなど他のツールの場合: `skill.md` はただのテキストファイルです。反パターンの表をプロジェクトルールに貼ったり、.mdc形式に変換したり、丸ごとコンテキストに食わせたりしてください。価値は具体的な置き換え例にあります。

## 実際に変わること

2018年頃のimportだらけではなく、現代の型ヒント:

```python
# 多くのモデルが今でも出力するコード
from typing import List, Optional, Dict
def process(items: List[str]) -> Optional[Dict[str, int]]: ...

# このスキルが出すコード
def process(items: list[str]) -> dict[str, int] | None: ...
```

思わず顔をしかめたくなるパス操作やループではなく:

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

完全な内容（構造的パターンマッチングとif-elifの使い分け、`dataclass(frozen=True, slots=True)`をいつ使うか、`asyncio.TaskGroup`、`zoneinfo`、25行以上の反パターン表、バージョンごとの機能表、10個の厳格ルール）はすべて [skill.md](skill.md) にあります。

## このスキルの狙い

Python 3.10以上を対象に、可能なら3.12以降の書き方を優先します。公開関数やモジュールレベルの名前には必ず本物の型アノテーションを付けます。stdlibが追いついた部分はサードパーティよりstdlibを優先。テストはpytestスタイルで、unittest.TestCaseは使いません。裸の`except:`、ミュータブルなデフォルト引数、理由のない`# type: ignore`は一切認めません。

目的はひとつです。古いStack Overflowの写しみたいなコード、AIが書いたと一目でわかるコードを世に出さなくすること。

実際にPythonを書いて仕事をしている人で、「またos.path.joinとtyping.Listか...」とため息をついたことがあるなら、このスキルはちょうどその不満を正すためのものです。

新しく広く使われるようになったイディオムがあれば、issueやPRで教えてください。それ以外はほぼskill.mdに書き切ってあります。

## ライセンス

MIT。詳細は [LICENSE](LICENSE) を参照。
