# pythonic

(English → README.md | 日本語 → README.ja.md | 简体中文 → README.zh-CN.md | 繁體中文 → README.zh-TW.md | Русский → README.ru.md)

Una skill para Grok que le hace escribir Python como lo haría un desarrollador senior de verdad, no la salida típica de un modelo.

## Instalación

**Uso personal** (se carga en todas las conversaciones):

```powershell
git clone https://github.com/drkai-lab/pythonic.git "$env:USERPROFILE\.grok\skills\pythonic"
```

**Dentro de un proyecto** (para un repositorio concreto o un equipo):

```powershell
# ejecútalo desde la raíz de tu repositorio
git clone https://github.com/drkai-lab/pythonic.git .\.grok\skills\pythonic
```

No hace falta ninguna herramienta especial. Un git clone normal basta. Después de añadirlo, recarga las skills en Grok.

Para Claude Code, Cursor, Windsurf u otras herramientas: `skill.md` es texto plano. Puedes pegar la tabla de anti-patrones en las reglas del proyecto, convertirla a formato .mdc o pasarla directamente al modelo. Lo que de verdad importa son los reemplazos concretos.

## Qué cambia en la práctica

Type hints modernos en lugar de las importaciones de 2018:

```python
# lo que la mayoría de modelos siguen soltando
from typing import List, Optional, Dict
def process(items: List[str]) -> Optional[Dict[str, int]]: ...

# lo que produce esta skill
def process(items: list[str]) -> dict[str, int] | None: ...
```

Manejo de rutas e iteración que no te haga poner cara de dolor:

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

El contenido completo (pattern matching estructural frente a cadenas de if-elif, cuándo usar `dataclass(frozen=True, slots=True)`, `asyncio.TaskGroup`, `zoneinfo`, la tabla de más de 25 anti-patrones, la matriz de características por versión y las diez reglas de obligado cumplimiento) está todo en [skill.md](skill.md).

## Qué hace realmente esta skill

Apunta a Python 3.10+ y usa por defecto los modismos de 3.12+ cuando existen. Todas las funciones públicas y nombres a nivel de módulo llevan anotación de tipo de verdad. Cuando la biblioteca estándar ya puede, se prefiere sobre paquetes de terceros. Tests en estilo pytest, nunca unittest.TestCase. Nada de `except:` desnudo, nada de defaults mutables, nada de `# type: ignore` sin una explicación decente.

El objetivo es sencillo: dejar de entregar código que parece sacado de un tutorial de 2018 o generado por un modelo vago.

Si eres desarrollador de Python que trabaja con él a diario y alguna vez has suspirado al ver que la IA te suelta `os.path.join` y `typing.List`, esta skill es la lente correctora.

Si ves hábitos nuevos que ya se han vuelto normales en bases de código reales, abre un issue o manda un PR. Lo demás ya está escrito en skill.md.

## Licencia

MIT. Consulta [LICENSE](LICENSE).
