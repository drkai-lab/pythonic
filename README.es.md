(English → README.md | 日本語 → README.ja.md | 简体中文 → README.zh-CN.md | 繁體中文 → README.zh-TW.md | Русский → README.ru.md)

# pythonic

Guía estricta para Python 3.10+ (preferiblemente 3.12+). Enfocada en seguridad de tipos, uso prioritario de la librería estándar y código que un desarrollador senior aceptaría sin problemas en una revisión. Diseñada para eliminar ese código "parece generado por IA" que funciona ahora pero se vuelve un dolor de cabeza después.

## Instalación

**Personal** (en tu máquina, para todos los proyectos):

PowerShell:
```powershell
git clone https://github.com/drkai-lab/pythonic $env:USERPROFILE\.grok\skills\pythonic
```

bash / zsh:
```bash
git clone https://github.com/drkai-lab/pythonic ~/.grok/skills/pythonic
```

**Por proyecto** (incluido en el repositorio para que todo el equipo use las mismas reglas):

```powershell
# PowerShell — comandos separados, sin &&
mkdir -p .grok\skills
git clone --depth 1 https://github.com/drkai-lab/pythonic .grok\skills\pythonic
```

En Unix solo cambia las barras. El clone manual funciona igual.

Para otras herramientas (Claude Code, Cursor, etc.): `skill.md` es texto plano. Adapta manualmente las reglas de enforcement y la tabla de anti-patrones.

## Un ejemplo real de antes/después

Código típico que generan las IAs:
```python
def process(data):
    result = []
    for item in data:
        if item > 0:
            result.append(item * 2)
    return result
```

Versión Pythonic:
```python
def process(items: list[float]) -> list[float]:
    return [x * 2 for x in items if x > 0]
```

Hay más de 30 ejemplos concretos en la tabla de anti-patrones de [skill.md](skill.md).

## Reglas que realmente se aplican (extracto)

- Todas las funciones y métodos deben llevar anotación de tipo de retorno.
- Operaciones con archivos: solo `pathlib`. Nada de `os.path`.
- Solo f-strings. Ni `%` ni `.format()`.
- Para dispatch por tipo o estructura se usa `match`. Cadenas largas de `isinstance` se consideran olor.
- Prohibido `except:` desnudo, argumentos por defecto mutables y `# type: ignore` sin justificación clara.

Si no se especifica la versión objetivo de Python, por defecto usa las construcciones de 3.12+ y pregunta cuando sea relevante.

El documento completo (17 secciones, tabla de características por versión y la gran tabla de anti-patrones) está en [skill.md](skill.md).

## Por qué existe esto

El Python que sale de los modelos de lenguaje suele parecer razonable a primera vista. Seis meses después, cuando hay que mantenerlo, casi siempre arrepientes. Esta skill es el conjunto de cosas que han causado dolor real en bases de código de verdad.

Es deliberadamente corto y directo. Si prefieres consejos suaves y vagas "mejores prácticas", usa otra skill.

## Contribuciones

Si encuentras un nuevo patrón de 3.12+ o un contraejemplo más claro, manda un PR. Los ejemplos deben ser cortos, reales y mantener el mismo tono seco. El "sin olor a IA" también aplica a la documentación.

## Licencia

MIT — ver [LICENSE](LICENSE).
