# pythonic

(English → README.md | 日本語はこちら → README.ja.md | 简体中文 → README.zh-CN.md | 繁體中文 → README.zh-TW.md | Español → README.es.md)

Заставляет агентов писать тот Python, который сеньор-разработчик реально выкатит в продакшен в 2026 году. Целевая версия — 3.10+, с сильным уклоном в возможности стандартной библиотеки 3.12+. Никакого мусора 2018 года из обучающих данных.

## Установка

Skill — это один файл. Каталог, в который его кладут, обязан называться `pythonic` (совпадает с `name:` во frontmatter).

**На уровне пользователя** (рекомендуется, доступен везде на машине):

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

**На уровне проекта** (коммитится вместе с репозиторием):

```bash
mkdir -p .grok/skills/pythonic
cp skill.md .grok/skills/pythonic/
```

Windows PowerShell:

```powershell
$target = Join-Path ".grok" "skills/pythonic"
New-Item -ItemType Directory -Force -Path $target | Out-Null
Copy-Item skill.md -Destination (Join-Path $target "skill.md")
```

Сначала клонируем:

```bash
git clone https://github.com/drkai-lab/pythonic.git
cd pythonic
# затем выполняем одну из команд копирования выше
```

Пользователи Claude Code могут положить тот же файл в `.claude/skills/pythonic/` (при необходимости переименовать в SKILL.md, если harness требует именно такое имя). В Cursor основные правила можно вручную перенести в `.cursor/rules` или `.mdc` — правок будет немного.

## Два конкретных примера

**Работа с файлами и путями (привычка к os.path, которая никак не умирает):**

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

**Аннотации типов (то, что агенты в 2026 году всё ещё косячат чаще всего):**

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

Полные правила, большая таблица антипаттернов, таблица доступности фич по версиям и жёсткий список требований — всё в [skill.md](skill.md). Обязательно прочитайте.

## Зачем это нужно

Без присмотра агенты спокойно выдают `from typing import List`, `os.path.join`, голый `except:`, семь уровней if/elif для проверки типов и `class Foo(object):`. Потому что бо́льшая часть их обучающих данных — это пятнадцатилетние туториалы и ответы со Stack Overflow.

Этот skill — постоянный корректор. В нём зафиксирован текущий консенсус Python core team и typing-sig, а также то, что реально проходит серьёзный код-ревью в продакшене.

## Вклад

Мы принимаем изменения, только если вы можете аргументированно объяснить, почему сеньор Python-разработчик, который последние два года выпускал код в продакшен, предпочёл бы новый вариант. Начните с чтения начала skill.md.

## Лицензия

MIT. Смотрите LICENSE.
