---
name: pythonic
description: "Write modern, Pythonic Python code targeting 3.10+ (prefer 3.12+). Enforces type safety, idiomatic patterns, and latest stdlib features. Use when the user says 'Python', 'pythonic', 'write Python code', 'Python implementation', or any task involving Python programming."
---

# Pythonic — Modern Python Code Skill

**Target**: Python 3.10+ (prefer 3.12+ features where applicable)
**Philosophy**: Idiomatic over clever. Explicit over implicit. Typed over dynamic.

Always write code that a senior Pythonista would approve of — clean, typed, leveraging modern stdlib, no AI slop.

---

## 1. Type Hints (Mandatory)

### Modern Syntax (3.10+)

```python
# ❌ Old: from typing import List, Dict, Optional, Union
from typing import Optional

# ✅ 3.10+: built-in generics
def process(items: list[str]) -> dict[str, int]: ...

# ✅ 3.10+: Union as `X | Y`, Optional as `X | None`
def find_user(uid: str | None) -> User | None: ...

# ✅ 3.12+: type parameter syntax (PEP 695)
def first[T](items: list[T]) -> T: ...

class Stack[T]:
    def push(self, item: T) -> None: ...
    def pop(self) -> T: ...
```

### TypedDict for structured dicts

```python
from typing import TypedDict

class UserDict(TypedDict):
    id: str
    name: str
    email: str
```

### Protocol for duck typing (prefer over ABC)

```python
from typing import Protocol, runtime_checkable

@runtime_checkable
class Drawable(Protocol):
    def draw(self) -> None: ...
```

### `Self` type (3.11+)

```python
from typing import Self

class Builder:
    def with_name(self, name: str) -> Self:
        self.name = name
        return self
```

### `Never` for exhaustiveness (3.11+)

```python
from typing import Never

def assert_never(value: Never) -> Never:
    raise AssertionError(f"Unexpected: {value}")
```

### `override` decorator (3.12+)

```python
from typing import override

class Child(Parent):
    @override
    def method(self) -> None: ...  # type error if parent doesn't have method
```

### `type` statement (3.12+)

```python
# type alias with proper scoping
type Vector = list[float]
type Handler[T] = Callable[[T], None]
```

---

## 2. Data Classes & Models

### `dataclasses` (stdlib, preferred for plain data)

```python
from dataclasses import dataclass, field, frozen

@dataclass(frozen=True, slots=True)  # slots=True requires 3.10+
class Point:
    x: float
    y: float
    label: str = ""
```

### `enum` with `StrEnum`/`IntEnum` (3.11+)

```python
from enum import StrEnum, auto

class Color(StrEnum):
    RED = auto()    # "RED"
    GREEN = auto()  # "GREEN"
```

### Named tuples (simple cases)

```python
from collections import namedtuple
from typing import NamedTuple

# ✅ Modern NamedTuple with type hints
class Coordinate(NamedTuple):
    lat: float
    lon: float
```

### Pydantic (when validation/serialization needed)

```python
from pydantic import BaseModel, Field

class User(BaseModel):
    id: str
    name: str = Field(min_length=1)
    tags: list[str] = []
```

---

## 3. Control Flow

### Structural Pattern Matching (3.10+)

```python
# Match type + structure
match value:
    case int() | float():
        print(f"Number: {value}")
    case str():
        print(f"String: {value}")
    case [first, *rest]:
        print(f"List: {first}, rest: {rest}")
    case _:
        print("Unknown")

# Match on dataclass
match point:
    case Point(0, 0):
        print("Origin")
    case Point(x, y):
        print(f"({x}, {y})")
```

### Walrus Operator (when it reduces nesting)

```python
# ✅ Good: avoids repeated computation
if (result := expensive_call()) is not None:
    process(result)

# ❌ Bad: walrus for no reason
if (x := get_value()) and (y := transform(x)):
    pass  # just use normal assignment
```

### Match statement vs. if-elif chains

```python
# ❌ if-elif chain for type dispatch
if isinstance(x, int): ...
elif isinstance(x, str): ...

# ✅ match for type dispatch (3.10+)
match x:
    case int(): ...
    case str(): ...
```

---

## 4. Strings & Formatting

### f-strings always (never % or .format())

```python
# ✅ Always this
name = f"{user.first} {user.last}"

# ✅ Debug format (3.8+)
print(f"{user=}")  # user=<User object>

# ✅ Format spec
price = f"{value:,.2f} USD"
```

### `str.removeprefix` / `str.removesuffix` (3.9+)

```python
# ✅ Modern
clean = filename.removeprefix("prefix_").removesuffix(".tmp")

# ❌ Old slicing tricks
if filename.startswith("prefix_"): clean = filename[7:]
```

---

## 5. Collections & Iteration

### dict operations (3.9+)

```python
# ✅ Merge (3.9+)
merged = dict1 | dict2

# ✅ Update (3.9+)
dict1 |= dict2
```

### Iterator tools

```python
# ✅ zip strict (3.10+)
for a, b in zip(list1, list2, strict=True):  # raises if unequal lengths
    ...

# ✅ batched (3.12+)
from itertools import batched
for batch in batched(items, n=4):
    ...

# ✅ pairwise (3.10+)
from itertools import pairwise
for prev, curr in pairwise(items):
    ...

# ✅ chain for flat iteration
from itertools import chain
for item in chain(list1, list2): ...
```

### Prefer list/dict/set comprehensions over map/filter

```python
# ✅ Pythonic
squares = [x**2 for x in range(10)]
evens = [x for x in items if x % 2 == 0]
lookup = {u.id: u for u in users}

# ❌ Avoid unless obvious
squares = list(map(lambda x: x**2, range(10)))
```

### `any()` / `all()` for boolean checks

```python
if any(item.is_valid() for item in items):
    ...
```

---

## 6. File & Path Operations

### `pathlib` always (never os.path)

```python
from pathlib import Path

# ✅ Modern
DATA_DIR = Path("data")
config = DATA_DIR / "config.json"
content = config.read_text()       # 3.5+
lines = config.read_text().splitlines()
config.write_text(new_content)
```

### tempfile for temporary files

```python
from tempfile import TemporaryDirectory

with TemporaryDirectory() as tmpdir:
    tmp = Path(tmpdir) / "work.txt"
    tmp.write_text(data)
    ...
```

---

## 7. Error Handling

### Exception groups (3.11+)

```python
try:
    ...
except* ValueError as e:
    ...
except* OSError as e:
    ...
```

### `raise from` for chaining

```python
try:
    ...
 except SomeError as e:
    raise BusinessError("context") from e
```

### Narrow except scope

```python
# ✅ Good: try around the minimal failing code
try:
    result = risky_operation()
except SpecificError:
    fallback()

# ❌ Bad: try wrapping too much
try:
    setup()
    result = risky_operation()
    cleanup()
except SpecificError:
    ...
```

---

## 8. Async & Concurrency

### `asyncio` with `TaskGroup` (3.11+)

```python
async def main() -> None:
    async with asyncio.TaskGroup() as tg:
        t1 = tg.create_task(fetch("a"))
        t2 = tg.create_task(fetch("b"))
    # both done here, exceptions propagate

asyncio.run(main())
```

### `asyncio.Runner` (3.11+)

```python
with asyncio.Runner() as runner:
    runner.run(main())
```

### Timeouts (3.11+)

```python
async with asyncio.timeout(10):
    result = await slow_operation()
```

### Synchronization

- `asyncio.Semaphore` for rate limiting
- `asyncio.Lock` for shared state
- `queue.Queue` for producer-consumer (or `asyncio.Queue`)

---

## 9. Time & Date

### `zoneinfo` (3.9+, stdlib replacement for pytz)

```python
from zoneinfo import ZoneInfo
from datetime import datetime, timezone

tokyo = ZoneInfo("Asia/Tokyo")
now = datetime.now(tokyo)

# UTC
utc_now = datetime.now(timezone.utc)
```

---

## 10. Function Design

### Single responsibility, pure where possible

```python
# ✅ Good: one thing, tested, typed
def compute_score(items: list[float]) -> float:
    if not items:
        return 0.0
    return sum(items) / len(items)

# ❌ Bad: does too much
def process(data):
    # validates, transforms, saves, emails — all in one
    ...
```

### Keyword-only and positional-only arguments

```python
def create_user(name: str, /, *, age: int | None = None) -> User:
    """name is positional-only, age is keyword-only."""
    ...

# Usage
create_user("Alice", age=30)   # ✅
create_user(name="Alice")       # ❌ name must be positional
create_user("Alice", 30)        # ❌ age must be keyword
```

### Proper use of `*args`, `**kwargs`

```python
def log(message: str, *values: object, level: str = "INFO") -> None:
    ...
```

---

## 11. Imports

### Standard order: stdlib → third-party → local

```python
import os
from collections.abc import Sequence
from pathlib import Path

import requests
from pydantic import BaseModel

from myapp.models import User
from myapp.utils import retry
```

### `collections.abc` over `typing` for abstract types

```python
from collections.abc import Sequence, Mapping, Iterable

# ✅ Use ABCs from collections.abc
def process(items: Sequence[str]) -> None: ...

# ❌ Don't use typing's deprecated versions
from typing import Sequence  # deprecated in 3.9+
```

---

## 12. Context Managers

### `contextlib.contextmanager` for simple cases

```python
from contextlib import contextmanager

@contextmanager
def managed_resource(path: str):
    resource = acquire(path)
    try:
        yield resource
    finally:
        release(resource)
```

### `contextlib.suppress` for ignoring known exceptions

```python
from contextlib import suppress

with suppress(FileNotFoundError):
    Path("temp.txt").unlink()
```

### `contextlib.chdir` (3.11+)

```python
from contextlib import chdir

with chdir("/tmp"):
    ...  # all operations happen in /tmp
```

### `contextlib.nullcontext` for conditional context managers

```python
from contextlib import nullcontext

ctx = profiler if enabled else nullcontext()
with ctx:
    ...
```

---

## 13. Testing

### `pytest` style (never unittest.TestCase)

```python
# ✅ pytest — plain assert
def test_compute_score() -> None:
    assert compute_score([1.0, 2.0, 3.0]) == 2.0
    assert compute_score([]) == 0.0

# ✅ parametrize
@pytest.mark.parametrize(
    ("items", "expected"),
    [
        ([1.0, 2.0, 3.0], 2.0),
        ([], 0.0),
        ([5.0], 5.0),
    ],
)
def test_compute_score_param(items: list[float], expected: float) -> None:
    assert compute_score(items) == expected
```

### Fixtures over setup/teardown

```python
@pytest.fixture
def db() -> Database:
    db = Database(":memory:")
    db.create_tables()
    yield db
    db.close()

def test_user_creation(db: Database) -> None:
    user = db.create_user("Alice")
    assert user.name == "Alice"
```

### `tmp_path` fixture (built-in)

```python
def test_file_processing(tmp_path: Path) -> None:
    f = tmp_path / "data.txt"
    f.write_text("hello")
    assert process_file(f) == "HELLO"
```

---

## 14. What NOT to Do (Anti-patterns)

| Anti-pattern | Modern replacement |
|---|---|
| `os.path.join(...)` | `Path(...) / ...` |
| `pytz` | `zoneinfo.ZoneInfo` (stdlib) |
| `from typing import List, Dict` | built-in `list[str]`, `dict[str, int]` |
| `%s` or `.format()` | f-strings |
| `map(lambda x: ..., items)` | comprehension |
| `filter(lambda x: ..., items)` | comprehension with `if` |
| `typing.Optional[X]` | `X \| None` |
| `typing.Union[X, Y]` | `X \| Y` |
| `except:` (bare) | `except Exception:` |
| `class MyClass(object):` | `class MyClass:` |
| `if key in dict.keys():` | `if key in dict:` |
| `open(f).read()` | `Path(f).read_text()` |
| `time.sleep()` in asyncio | `await asyncio.sleep()` |
| `unittest.TestCase` | `pytest` function style |
| `with open(...) as f` for text files | `Path.read_text()` / `Path.write_text()` |
| `sys.stdin.read()` / `sys.argv` | `click` / `typer` / `argparse` |
| manual `__init__` boilerplate | `@dataclass` |
| ABC for simple interfaces | `Protocol` |
| `random.seed(time())` | `random.seed()` (auto-seeded in 3.2+) |
| `asyncio.get_event_loop().run_until_complete(...)` | `asyncio.run(...)` |
| `async with aiofiles.open(...)` | `anyio` or just `Path.read_text()` for small files |
| type comments (`# type:`) | inline type annotations |

---

## 15. Version-Conscious Feature Gates

When writing code and you don't know the target Python version, the skill must ask. Otherwise:

| Feature | Min Python |
|---|---|
| `functools.cache` | 3.9 |
| `str.removeprefix` / `str.removesuffix` | 3.9 |
| `zoneinfo` | 3.9 |
| dict `\|` merge | 3.9 |
| `match` / `case` | 3.10 |
| `dataclass(slots=True)` | 3.10 |
| `zip(strict=True)` | 3.10 |
| `itertools.pairwise` | 3.10 |
| `X \| Y` union syntax | 3.10 |
| `ExceptionGroup` / `except*` | 3.11 |
| `Self` type | 3.11 |
| `asyncio.TaskGroup` | 3.11 |
| `asyncio.timeout` | 3.11 |
| `contextlib.chdir` | 3.11 |
| `Never` type | 3.11 |
| `StrEnum` / `IntEnum` | 3.11 |
| `tomllib` | 3.11 |
| `type` statement (PEP 695) | 3.12 |
| `@override` | 3.12 |
| `itertools.batched` | 3.12 |
| `Path.walk()` | 3.12 |
| `collections.defaultdict` `default_factory` improvements | 3.12 |
| `@dataclass(frozen=True, kw_only=True)` (kw_only in 3.10+, but frozen + slots + kw_only all together is 3.12 best) | 3.12 |

When writing for an unknown-version target, default to **3.12+** unless constraints dictate otherwise.

---

## 16. Enforcement Rules (always apply)

1. **All functions, methods, and module-level variables MUST have type annotations.** No bare `def foo():` without `-> None` or actual return type.
2. **No `# type: ignore` without a comment explaining why.** The comment must reference a ticket or the specific mypy limitation.
3. **No `Any` unless absolutely unavoidable** (e.g., deserializing unknown JSON). Use `object`, `Unknown`, or a `Protocol` first.
4. **No unused imports or variables.** Lint-clean or bust.
5. **No mutable default arguments.** Use `None` + `field(default_factory=...)` or `None` guard.
6. **No bare `except:`.** Always `except Exception:` or specific exception types.
7. **Generator functions must be annotated with their yield type.**
8. **Abstract base classes use `ABC` from `abc` only when `Protocol` from `typing` doesn't suffice.**
9. **`__init__.py` files stay minimal** — re-export public API only.
10. **`__all__` defined in every `__init__.py`** for explicit public API.

---

## 17. Reference: Pythonic Idioms Cheatsheet

```python
# Swap
a, b = b, a

# Unpacking
first, *middle, last = items

# Negative indexing
last_item = items[-1]

# enumerate with start
for i, item in enumerate(items, start=1): ...

# dict.get with default
value = cache.get(key, DEFAULT)

# dict.setdefault for list-of-lists
groups: dict[str, list[int]] = {}
groups.setdefault(key, []).append(value)

# collections.Counter
from collections import Counter
counts = Counter(items)
most_common = counts.most_common(3)

# collections.defaultdict
from collections import defaultdict
by_type: dict[str, list[Item]] = defaultdict(list)
by_type[kind].append(item)

# functools.cache / lru_cache
from functools import cache

@cache
def fibonacci(n: int) -> int: ...

# singledispatch for type-based dispatch
from functools import singledispatch

@singledispatch
def serialize(obj: object) -> str: ...

@serialize.register
def _(obj: int) -> str: return f"int:{obj}"

@serialize.register
def _(obj: list) -> str: return f"list:{len(obj)}"

# Context managers for resource cleanup
from contextlib import closing

with closing(create_connection()) as conn:
    conn.query(...)

# Ellipsis for stub/marker
def todo() -> None: ...  # not pass

# Conditional expression (ternary) — use sparingly
label = "active" if user.is_active else "inactive"
```
