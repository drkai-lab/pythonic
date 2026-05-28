# pythonic

(English → README.md | 日本語はこちら → README.ja.md | 简体中文 → README.zh-CN.md | 繁體中文 → README.zh-TW.md | Русский → README.ru.md)

Obliga a los agentes a escribir el Python que un desarrollador senior realmente pondría en producción en 2026. Apunta a 3.10+, con preferencia fuerte por las características de la biblioteca estándar de 3.12+. Nada de código viejo de 2018 sacado de los datos de entrenamiento.

## Instalación

La skill es un único archivo. El directorio donde se coloca debe llamarse exactamente `pythonic` (coincide con el `name:` del frontmatter).

**Nivel de usuario** (recomendado, disponible en toda la máquina):

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

**Nivel de proyecto** (se commitea junto con el repositorio):

```bash
mkdir -p .grok/skills/pythonic
cp skill.md .grok/skills/pythonic/
```

Equivalente en Windows PowerShell:

```powershell
$target = Join-Path ".grok" "skills/pythonic"
New-Item -ItemType Directory -Force -Path $target | Out-Null
Copy-Item skill.md -Destination (Join-Path $target "skill.md")
```

Primero clona:

```bash
git clone https://github.com/drkai-lab/pythonic.git
cd pythonic
# luego ejecuta uno de los comandos de copia de arriba
```

Los usuarios de Claude Code pueden colocar el mismo archivo en `.claude/skills/pythonic/` (renómbralo a SKILL.md si tu harness es estricto con el nombre). En Cursor puedes extraer las reglas principales a `.cursor/rules` o archivos `.mdc` con muy pocos cambios.

## Dos ejemplos concretos

**Manejo de archivos y rutas (el vicio de os.path que se niega a morir):**

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

**Anotaciones de tipo (lo que los agentes siguen fallando más en 2026):**

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

Todas las reglas completas, la gran tabla de anti-patrones, las puertas de versión y la lista de obligaciones están en [skill.md](skill.md). Léelo.

## Por qué existe

Si se les deja solos, los agentes escupen `from typing import List`, `os.path.join`, `except:` desnudo, cadenas de if/elif de siete niveles para comprobar tipos y `class Foo(object):`. Porque la mayor parte de sus datos de entrenamiento son tutoriales y respuestas de Stack Overflow de hace quince años.

Esta skill es el correctivo permanente. Codifica el consenso actual del equipo de Python, el typing-sig y lo que realmente sobrevive a una revisión seria de código en producción.

## Contribuir

Solo aceptamos cambios si puedes defender que un desarrollador senior de Python que ha sacado código a producción en los últimos dos años preferiría la versión nueva. Empieza leyendo el principio de skill.md.

## Licencia

MIT. Consulta LICENSE.
