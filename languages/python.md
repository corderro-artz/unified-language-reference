# Python

`multi-paradigm` · `dynamic, strong, duck-typed` · `interpreted (bytecode VM)` · `1991`

## Contents

- **Identity**
  - [Overview](#overview)
  - [Language Type](#language-type)
  - [Paradigms](#paradigms)
  - [Mental Model](#mental-model)
- **Foundations**
  - [Lexical & Syntax](#lexical--syntax)
  - [Variables & Bindings](#variables--bindings)
  - [Type System](#type-system)
  - [Data Structures](#data-structures)
  - [Operators & Expressions](#operators--expressions)
- **Logic**
  - [Control Flow](#control-flow)
  - [Functions](#functions)
  - [Error Handling](#error-handling)
- **Abstraction**
  - [Object Model](#object-model)
  - [Functional Constructs](#functional-constructs)
  - [Modules & Namespaces](#modules--namespaces)
  - [Metaprogramming & Reflection](#metaprogramming--reflection)
- **Runtime**
  - [Memory Management](#memory-management)
  - [Concurrency & Parallelism](#concurrency--parallelism)
- **Practice**
  - [Standard Library — Start Here](#standard-library--start-here)
  - [Tooling & Ecosystem](#tooling--ecosystem)
  - [Conventions & Style](#conventions--style)
  - [Idioms & Gotchas](#idioms--gotchas)
- **Reference**
  - [Versioning & Editions](#versioning--editions)
  - [Resources](#resources)

---

## Identity

### Overview

Python is a general-purpose, dynamically typed language run by an
interpreter (CPython) that compiles source to bytecode and executes
it on a virtual machine. It runs cross-platform on Windows, Linux,
and macOS, and powers web back ends (Django, Flask, FastAPI),
data science and machine learning (NumPy, pandas, PyTorch),
scripting, and automation. It is known for readability, a
"batteries-included" standard library, and a vast third-party
ecosystem.

### Language Type

| Axis      | This language                              |
|-----------|--------------------------------------------|
| Execution | interpreted to bytecode, run on CPython VM |
| Domain    | general-purpose                            |
| Typing    | dynamic, strong, duck-typed; optional hints |
| Memory    | managed; reference counting + cyclic GC    |

### Paradigms

- **Imperative** — a program is a sequence of statements that change state. Here: the default statement-by-statement flow.
- **Procedural** — imperative code organized into reusable procedures. Here: `def` functions and modules group behavior.
- **Object-Oriented** — bundles state and behavior into types; objects interact through methods. Here: everything is an object; classes, inheritance, and dunder methods.
- **Functional** — computation as the evaluation of functions; values over mutation, functions as first-class data. Here: lambdas, closures, comprehensions, `functools`/`itertools`.
- **Concurrent** — work structured as independently progressing tasks. Here: `async`/`await`, threads, and multiprocessing.
- **Metaprogramming** — programs that read, generate, or transform code. Here: decorators, metaclasses, descriptors, and runtime introspection.

### Mental Model

Internalize the Zen of Python (PEP 20): there should be one obvious
way to do it, readability counts, and explicit beats implicit.
Everything is an object — including integers, functions, and classes
— and behavior is decided by duck typing ("if it quacks like a
duck"), not declared types. Prefer EAFP ("easier to ask forgiveness
than permission") over LBYL: try the operation and catch the
exception. Indentation is not cosmetic — it delimits blocks and is
part of the grammar.

## Foundations

### Lexical & Syntax

- **Comments** — `#` to end of line; there is no block-comment
  syntax. A triple-quoted string as the first statement of a module,
  class, or function is its docstring.
- **Statements** — newline-terminated; `;` between statements is
  legal but rare. Indentation (not braces) delimits blocks.
- **Keywords** — lowercase (`def`, `class`, `import`); the constants
  `True`, `False`, and `None` are capitalized.
- **Entry point** — top-level module code runs on import or
  execution; guard a script's main with
  `if __name__ == "__main__":`.
- **File structure** — each `.py` file is a module; `import`
  statements conventionally sit at the top.

```python
if __name__ == "__main__":
    print("Hello, World!")
```

<details>
<summary>Examples — one per item</summary>

```python
x = 1  # Comments — line only
"""Module docstring."""

a = 1; b = 2     # Statements — ; optional
if a:            # indentation delimits
    pass

def f():         # Keywords — lowercase
    return None  # True/False/None capitalized

# Entry point
if __name__ == "__main__":
    main()

# File structure — a .py file is a module
import os
```

</details>

### Variables & Bindings

- **Assignment** — `x = 5` binds a name to an object; names are
  references and need no declaration.
- **No true constants** — convention is `UPPER_SNAKE`;
  `typing.Final` documents intent but is not enforced at runtime.
- **Scope** — name lookup follows LEGB (Local, Enclosing, Global,
  Built-in); `global` and `nonlocal` rebind outer names.
- **Multiple / augmented** — `a, b = 1, 2` unpacks; `x += 1` is
  augmented assignment.
- **Walrus** — `:=` assigns within an expression (3.8+).

```python
x = 5
MAX_LEN = 100
a, b = 1, 2
```

<details>
<summary>Examples — one per item</summary>

```python
x = 5                # Assignment — a reference

MAX_LEN = 100        # No true constants
from typing import Final
LIMIT: Final = 10

def outer():         # Scope — LEGB
    n = 0
    def inner():
        nonlocal n
        n += 1
    inner()

a, b = 1, 2          # Multiple / augmented
a += 1

if (n := len("ab")): # Walrus (3.8+)
    print(n)
```

</details>

### Type System

| Syntax     | Type           | Size      | Range                  | Default | Literal |
|------------|----------------|-----------|------------------------|---------|---------|
| `int`      | integer        | arbitrary | unbounded              | `0`     | `42`    |
| `float`    | float          | 64-bit    | IEEE-754 double        | `0.0`   | `3.14`  |
| `complex`  | complex        | 2×64-bit  | IEEE-754 real + imag   | `0j`    | `1+2j`  |
| `bool`     | boolean        | n/a       | `False` / `True`       | `False` | `True`  |
| `str`      | text string    | n/a       | Unicode code points    | `''`    | `"hi"`  |
| `bytes`    | byte string    | n/a       | 0..255 per byte        | `b''`   | `b"hi"` |
| `NoneType` | unit / null    | n/a       | the singleton `None`   | `None`  | `None`  |

<details>
<summary>Other built-in scalar-ish types</summary>

| Syntax      | Type          | Size | Range           | Default          | Literal           |
|-------------|---------------|------|-----------------|------------------|-------------------|
| `bytearray` | mutable bytes | n/a  | 0..255 per byte | `bytearray(b'')` | `bytearray(b"a")` |

`int` is arbitrary-precision: it never overflows, growing with
available memory. `bool` subclasses `int`, so `True == 1` and
`False == 0`. Collection types (`list`, `dict`, `set`, `tuple`)
live in [Data Structures](#data-structures).
</details>

> [!NOTE]
> Everything in Python is an object — `int`, `str`, functions, and
> classes alike. There are no unboxed primitives; `type(x)` gives an
> object's runtime type, and all types ultimately subclass `object`.

- **Inference** — there is no static inference; a name's type is
  whatever object it currently refers to, checked at runtime.
- **Conversion** — no implicit numeric widening beyond
  `int`→`float`→`complex` in mixed arithmetic; convert explicitly
  with `int()`, `float()`, `str()`.
- **Nullability** — absence is the `None` singleton; test with
  `x is None`. Hint optional values as `Optional[T]` or `T | None`.
- **Type hints / Generics** — annotations are gradual (PEP 484) and
  runtime-erased; PEP 695 (3.12+) adds `def f[T](x: T) -> T`. In
  3.14 annotations are evaluated lazily (PEP 649/749).

```python
x = 42            # runtime type int
y: int | None = None
def first[T](xs: list[T]) -> T:
    return xs[0]
```

<details>
<summary>Examples — one per item</summary>

```python
n = 42              # Inference — runtime type

x = int("3")        # Conversion — explicit

y: int | None = None  # Nullability
if y is None:
    pass

def first[T](        # Type hints / Generics
    xs: list[T]
) -> T:
    return xs[0]
```

</details>

### Data Structures

| Structure   | Syntax              | Ordered | Mutable | Use                |
|-------------|---------------------|---------|---------|--------------------|
| `list`      | `[1, 2, 3]`         | yes     | yes     | growable sequence  |
| `tuple`     | `(1, 2)`            | yes     | no      | immutable group    |
| `dict`      | `{"a": 1}`          | yes     | yes     | key lookup         |
| `set`       | `{1, 2}`            | no      | yes     | unique membership  |
| `frozenset` | `frozenset({1})`    | no      | no      | immutable set      |
| `str`       | `"abc"`             | yes     | no      | text sequence      |
| `bytes`     | `b"abc"`            | yes     | no      | binary sequence    |
| `bytearray` | `bytearray(b"a")`   | yes     | yes     | mutable binary     |

> [!NOTE]
> `dict` has preserved insertion order since 3.7. Reach into
> `collections` for `deque` (fast ends), `Counter` (tallying), and
> `defaultdict` (auto-initialized values).

### Operators & Expressions

| Category   | Operator | Name           | Example      | Note                  |
|------------|----------|----------------|--------------|-----------------------|
| Arithmetic | `+`      | add            | `a + b`      | also concatenates     |
| Arithmetic | `/`      | divide         | `a / b`      | always returns float  |
| Arithmetic | `//`     | floor divide   | `a // b`     | rounds toward -inf    |
| Arithmetic | `%`      | modulo         | `a % b`      | remainder             |
| Arithmetic | `**`     | power          | `a ** b`     | right-associative     |
| Comparison | `==`     | equal          | `a == b`     | value equality        |
| Comparison | `<`      | less-than      | `a < b < c`  | chains allowed        |
| Logical    | `and`    | and            | `a and b`    | short-circuits        |
| Logical    | `or`     | or             | `a or b`     | returns an operand    |
| Logical    | `not`    | not            | `not a`      | low precedence        |
| Bitwise    | `&`      | and            | `a & b`      |                       |
| Bitwise    | `\|`     | or             | `a \| b`     |                       |
| Bitwise    | `^`      | xor            | `a ^ b`      |                       |
| Bitwise    | `<<`     | left shift     | `a << 2`     |                       |
| Assignment | `=`      | assign         | `a = b`      |                       |
| Assignment | `+=`     | add-assign     | `a += b`     |                       |
| Assignment | `:=`     | walrus         | `(n := f())` | assigns in expression |
| Special    | `is`     | identity       | `a is None`  | same object?          |
| Special    | `in`     | membership     | `x in xs`    | contains?             |
| Special    | `@`      | matmul         | `a @ b`      | also decorator syntax |

- **Overloading** — operators map to dunder methods (`__add__`,
  `__eq__`, `__lt__`, `__matmul__`); define them to overload.

> [!NOTE]
> `**` is right-associative (`2 ** 3 ** 2 == 512`) and `not` binds
> looser than comparisons. The ternary is `a if cond else b`, and
> `*`/`**` unpack in calls and literals (`f(*args, **kwargs)`).

## Logic

### Control Flow

- **Conditionals** — `if` / `elif` / `else`.
- **`match`** — structural pattern matching (3.10+): cases match
  shapes, with capture and `if` guards.
- **Loops** — `for x in iterable` and `while cond`; both take an
  optional `else` that runs if no `break` fired.
- **Comprehensions** — `list`, `dict`, `set`, and generator forms
  build sequences inline.
- **Jumps** — `break`, `continue`, `pass`, `return`.
- **Context managers** — `with` scopes setup/teardown around a block.

```python
match point:
    case (0, 0):
        label = "origin"
    case (x, y) if x == y:
        label = "diagonal"
    case _:
        label = "other"
```

<details>
<summary>Examples — one per item</summary>

```python
if n > 0:            # Conditionals
    pass
elif n == 0:
    pass
else:
    pass

match cmd:           # match (3.10+)
    case "go":
        pass
    case _:
        pass

for x in [1, 2]:     # Loops
    pass
else:
    pass

evens = [             # Comprehensions
    n for n in xs if n % 2 == 0
]

# Jumps — break / continue / pass / return

with open("f") as fh:  # Context managers
    data = fh.read()
```

</details>

### Functions

- **Declaration** — `def name(params):`; the first string is a
  docstring.
- **Parameters** — positional, keyword, defaults, `*args`,
  `**kwargs`; keyword-only after `*`, positional-only before `/`.
- **Lambdas** — `lambda x: x + 1` is a single-expression function.
- **First-class** — functions are objects; nested functions form
  closures over enclosing locals.
- **Higher-order** — pass and return functions: `map`, `filter`,
  `sorted(key=...)`.
- **Generators** — `yield` produces lazy iterators; `yield from`
  delegates.
- **Decorators** — wrap a function with `@deco` (see
  [Metaprogramming](#metaprogramming--reflection)).

```python
def add(a, b=0, *args, **kwargs):
    return a + b
```

<details>
<summary>Examples — one per item</summary>

```python
def sq(x):           # Declaration
    """Square x."""
    return x * x

def p(a, b=0, *xs,   # Parameters
      **kw):
    return a

dbl = lambda x: x * 2  # Lambdas

def outer():         # First-class — closure
    n = 1
    return lambda: n

evens = filter(      # Higher-order
    lambda x: x % 2 == 0, xs
)

def seq():           # Generators
    yield 1
    yield from [2, 3]

@staticmethod        # Decorators
def helper():
    pass
```

</details>

### Error Handling

- **Exceptions** — `try` / `except` / `else` / `finally`; `raise`
  to throw. All exceptions derive from `BaseException`.
- **Exception groups** — `except*` matches members of an
  `ExceptionGroup` (3.11+, PEP 654).
- **Propagation** — an unhandled exception unwinds to the nearest
  matching `except`; `raise X from cause` chains the original.
- **Context managers** — `with` (and `contextlib`) guarantee
  cleanup even on error.
- **Assertions** — `assert cond, msg`; stripped when Python runs
  with `-O`.

```python
try:
    risky()
except ValueError as e:
    log(e)
finally:
    cleanup()
```

<details>
<summary>Examples — one per item</summary>

```python
try:                 # Exceptions
    risky()
except ValueError as e:
    log(e)
else:
    ok()
finally:
    cleanup()

try:                 # Exception groups (3.11+)
    work()
except* TypeError:
    pass

raise ValueError(    # Propagation
    "bad"
) from err

with open("f") as f: # Context managers
    f.read()

assert x > 0, "pos"  # Assertions
```

</details>

## Abstraction

### Object Model

- **Classes** — `class C:` defines a type; `__init__` initializes
  instances.
- **Instantiation** — `C(args)` calls `__new__` then `__init__`.
- **Members** — instance vs class attributes; methods; `@property`
  for computed access; `@staticmethod` / `@classmethod`.
- **Encapsulation** — by convention: `_protected` is internal,
  `__mangled` triggers name mangling; nothing is truly private.
- **Inheritance** — single and multiple; method resolution follows
  the C3 MRO; call up with `super()`.
- **Duck typing & protocols** — structural typing via
  `typing.Protocol`; abstract base classes via `abc`.
- **Polymorphism** — method calls dispatch dynamically on the
  object's type.
- **Dataclasses** — `@dataclass` synthesizes `__init__`, `__repr__`,
  and `__eq__`.

```python
from dataclasses import dataclass

@dataclass
class Point:
    x: int
    y: int
```

<details>
<summary>Examples — one per item</summary>

```python
class Animal:        # Classes
    def __init__(self, name):
        self.name = name

a = Animal("Rex")    # Instantiation

class Dog(Animal):   # Members
    @property
    def label(self):
        return self.name

# Encapsulation — _protected, __mangled
class Box:
    def __init__(self):
        self._cache = {}

class Pup(Dog):      # Inheritance — super / MRO
    def __init__(self, n):
        super().__init__(n)

from typing import Protocol
class Sized(Protocol):  # Duck typing
    def __len__(self) -> int: ...

# Polymorphism — dynamic dispatch
for x in (Dog("a"), Pup("b")):
    print(x.label)

from dataclasses import dataclass
@dataclass            # Dataclasses
class Pt:
    x: int
```

</details>

### Functional Constructs

- **Immutability** — `tuple`, `frozenset`, and `str` are immutable;
  `@dataclass(frozen=True)` freezes a class.
- **First-class functions** — lambdas, closures, and
  `functools.partial` for partial application.
- **Comprehensions / generators** — build sequences declaratively;
  generators are lazy.
- **`functools` / `itertools`** — `reduce`, `lru_cache`, `chain`,
  and other combinators.
- **Pattern matching** — `match` deconstructs and matches data
  shapes.
- **No native ADTs** — model sum types with `Enum`, `dataclass`,
  and `|` unions.

```python
from functools import reduce
total = reduce(lambda a, b: a + b, xs)
```

<details>
<summary>Examples — one per item</summary>

```python
p = (1, 2)           # Immutability
from dataclasses import dataclass
@dataclass(frozen=True)
class Pt:
    x: int

from functools import partial
inc = partial(lambda a, b: a + b, 1)  # First-class

squares = (           # Comprehensions / generators
    n * n for n in xs
)

from functools import reduce  # functools / itertools
total = reduce(lambda a, b: a + b, xs)

match cmd:            # Pattern matching
    case ["go", x]:
        pass

from enum import Enum  # No native ADTs
class Color(Enum):
    RED = 1
```

</details>

### Modules & Namespaces

- **Module** — a `.py` file; `import m`, `from m import x`, and
  `import m as alias`.
- **Package** — a directory of modules (with `__init__.py`, or a
  namespace package without one).
- **Visibility** — no enforcement; `__all__` controls `from m
  import *`, and a leading `_` signals "internal".
- **Packaging** — distribute via pip and PyPI; configure with
  `pyproject.toml`; ship wheels.
- **Import system** — modules are found along `sys.path`; isolate
  dependencies in virtual environments.

```python
import os
from collections import deque
import numpy as np
```

<details>
<summary>Examples — one per item</summary>

```python
import os            # Module
from os import path
import os as o

# Package — dir with __init__.py
from mypkg.sub import thing

__all__ = ["public"]  # Visibility

# Packaging — pip + PyPI + pyproject.toml
# pip install requests

import sys            # Import system
sys.path.append(".")
```

</details>

### Metaprogramming & Reflection

- **Reflection / introspection** — `type`, `getattr` / `setattr` /
  `hasattr`, `dir`, and the `inspect` module read objects at
  runtime.
- **Decorators** — `@deco` wraps a function or class to extend it.
- **Dunder methods** — `__add__`, `__iter__`, `__call__` and peers
  customize built-in behavior.
- **Metaclasses** — a class whose instances are classes; subclass
  `type` to control class creation.
- **`eval` / `exec` / `compile`** — execute code built at runtime
  from strings.
- **Descriptors** — objects defining `__get__` / `__set__` that
  govern attribute access (the basis of `property`).

```python
def trace(fn):
    def wrap(*a, **k):
        return fn(*a, **k)
    return wrap
```

<details>
<summary>Examples — one per item</summary>

```python
t = type(obj)        # Reflection / introspection
v = getattr(obj, "x", None)

def deco(fn):        # Decorators
    return fn

class C:             # Dunder methods
    def __call__(self):
        return 1

class Meta(type):    # Metaclasses
    pass

exec("y = 1")        # eval / exec / compile

class Field:         # Descriptors
    def __get__(self, obj, owner):
        return 1
```

</details>

> [!WARNING]
> `eval`/`exec` run arbitrary code — never feed them untrusted
> input. Metaclasses and descriptors are powerful but obscure;
> prefer decorators or `__init_subclass__` for most needs.

## Runtime

### Memory Management

- **Reference counting** — objects are freed immediately when their
  reference count reaches zero.
- **Cyclic GC** — a generational garbage collector (`gc`) reclaims
  reference cycles.
- **No manual free** — memory is managed; `del` only drops a
  name/reference, not the object directly.
- **References, not pointers** — names bind to objects; no pointer
  arithmetic.
- **`__del__` finalizers** — a non-deterministic cleanup hook; not
  guaranteed to run promptly.
- **`weakref`** — weak references that don't keep an object alive.

<details>
<summary>Examples — one per item</summary>

```python
x = []               # Reference counting
y = x                # refcount now 2
del y                # drops a name, not object

import gc            # Cyclic GC
gc.collect()

del x                # No manual free — del a name

a = [1]              # References, not pointers
b = a                # both name one list

class R:             # __del__ finalizers
    def __del__(self):
        pass

import weakref       # weakref
ref = weakref.ref(R())
```

</details>

> [!NOTE]
> This memory model is CPython-specific. Reference counting frees
> objects immediately at zero references; a generational cyclic
> collector (`gc`) reclaims reference cycles. `del` removes a name
> binding, not the object. `__del__` finalizers run
> non-deterministically — use `weakref` and context managers
> instead of relying on them. Other implementations (e.g. PyPy)
> use a tracing GC and behave differently.

### Concurrency & Parallelism

- **Threads** — `threading` gives OS threads; in default CPython
  they are bound by the GIL for CPU-heavy work.
- **The GIL** — the Global Interpreter Lock lets only one thread
  execute Python bytecode at a time in the default build; I/O
  releases it.
- **Free-threaded build** — 3.14 makes the GIL-free build officially
  supported (PEP 779), but the GIL stays enabled by default in the
  standard build.
- **`asyncio`** — `async`/`await` over a single-threaded event loop
  for high-concurrency I/O.
- **`multiprocessing`** — separate processes give true CPU
  parallelism, sidestepping the GIL.
- **`concurrent.futures`** — `ThreadPoolExecutor` and
  `ProcessPoolExecutor` over a uniform `Future` API.
- **Synchronization** — `Lock`, `Queue`, `Event`, and friends
  coordinate threads.
- **Subinterpreters** — `concurrent.interpreters` runs isolated
  interpreters in one process (3.14, PEP 734).

```python
import asyncio

async def main():
    await asyncio.sleep(1)
```

<details>
<summary>Examples — one per item</summary>

```python
import threading     # Threads
threading.Thread(target=work).start()

# The GIL — one thread runs bytecode at once

# Free-threaded build — python3.14t (PEP 779)

import asyncio       # asyncio
async def main():
    await asyncio.sleep(1)

from multiprocessing import Process  # multiprocessing
Process(target=work).start()

from concurrent.futures import (    # concurrent.futures
    ThreadPoolExecutor,
)
with ThreadPoolExecutor() as ex:
    ex.submit(work)

lock = threading.Lock()  # Synchronization

from concurrent import interpreters  # Subinterpreters
```

</details>

> [!WARNING]
> The GIL means CPU-bound threads do not run in parallel in the
> default build. For true parallelism use `multiprocessing`, or the
> supported free-threaded build (`python3.14t`).

## Practice

### Standard Library — Start Here

The standard library ships with CPython — batteries included, no
install needed. Pull a module in with `import name` and use it.

| Module        | Purpose                              | Import                       |
|---------------|--------------------------------------|------------------------------|
| `os`          | operating-system interfaces          | `import os`                  |
| `sys`         | interpreter parameters               | `import sys`                 |
| `pathlib`     | object-oriented filesystem paths     | `from pathlib import Path`   |
| `collections` | extra container datatypes            | `import collections`         |
| `itertools`   | iterator-building combinators        | `import itertools`           |
| `functools`   | higher-order function tools          | `import functools`           |
| `json`        | JSON encode / decode                 | `import json`                |
| `re`          | regular expressions                  | `import re`                  |
| `datetime`    | dates and times                      | `from datetime import date`  |
| `math`        | mathematical functions and constants | `import math`                |
| `typing`      | type-hint support                    | `from typing import Any`     |
| `dataclasses` | boilerplate-free data classes        | `from dataclasses import *`  |
| `asyncio`     | asynchronous I/O                     | `import asyncio`             |
| `unittest`    | unit-testing framework               | `import unittest`            |
| `logging`     | logging facility                     | `import logging`             |

### Tooling & Ecosystem

| Tool             | Role                          | Invoke                  |
|------------------|-------------------------------|-------------------------|
| CPython          | reference interpreter         | `python`                |
| pip              | package installer             | `pip install X`         |
| venv             | virtual environments          | `python -m venv .venv`  |
| `pyproject.toml` | project / build config        | (file)                  |
| ruff             | linter + formatter            | `ruff check`            |
| black            | code formatter                | `black .`               |
| mypy             | static type checker           | `mypy .`                |
| pytest           | test runner                   | `pytest`                |
| REPL             | interactive shell             | `python`                |
| pdb              | debugger                      | `python -m pdb app.py`  |
| uv               | fast env + dependency manager | `uv run app.py`         |
| Django / FastAPI | dominant web frameworks       | `pip install fastapi`   |

### Conventions & Style

| Identifier           | Convention            | Example      |
|----------------------|-----------------------|--------------|
| functions / variables | snake_case           | `user_id`    |
| classes              | PascalCase            | `HttpClient` |
| constants            | UPPER_SNAKE           | `MAX_LEN`    |
| modules / packages   | lowercase / snake_case | `my_module` |
| "private"            | `_leading_underscore` | `_cache`     |
| name-mangled         | `__double_leading`    | `__secret`   |
| magic                | `__dunder__`          | `__init__`   |

- **Formatting** — four-space indentation, no tabs; follow PEP 8.
  `black` and `ruff` enforce style automatically.
- **Project layout** — application code under `src/`, tests under
  `tests/`, configuration in `pyproject.toml`.
- **Docstrings** — triple-quoted strings document modules, classes,
  and functions per PEP 257.

Typical `src/` project layout — package under `src/`, tests in a
parallel `tests/`:

```text
myproject/
├─ src/                     application code
│  └─ mypkg/                the package
│     ├─ __init__.py
│     └─ core.py
├─ tests/                   test suite
│  └─ test_core.py
├─ pyproject.toml           project + build config
├─ README.md
└─ .gitignore
```

Official style guide: <https://peps.python.org/pep-0008/>

### Idioms & Gotchas

- **EAFP** — try the operation and catch the exception rather than
  pre-checking conditions.
- **Comprehensions** — prefer `[f(x) for x in xs]` over manual
  append loops.
- **f-strings** — interpolate with `f"{name}={value}"`.
- **Context managers** — use `with` for files, locks, and any
  acquire/release resource.

> [!WARNING]
> Mutable default arguments are evaluated once: `def f(x=[])`
> shares one list across calls. Use `def f(x=None)` and assign
> inside.

> [!WARNING]
> Closures capture variables, not values: lambdas built in a loop
> all see the loop variable's final value. Bind it with a default
> argument.

> [!WARNING]
> `is` tests identity, `==` tests equality. Use `is` only for
> `None`, `True`, `False`, and other singletons.

> [!WARNING]
> The GIL prevents CPU-bound threads from running in parallel in
> the default build — reach for `multiprocessing` instead.

## Reference

### Versioning & Editions

Python uses `MAJOR.MINOR.PATCH` versioning with one feature release
each year. A release receives roughly two years of bugfix updates
and about five years of security support. **Python 3.14 is the
current feature line** (released October 2025; latest patch 3.14.6,
June 2026). Python 2 reached end of life in 2020, so all current
development targets Python 3. The 3.14 line makes the free-threaded
build officially supported (PEP 779) and ships an experimental JIT.

> [!NOTE]
> Python 3.14 highlights: deferred evaluation of annotations
> (PEP 649/749), template strings / t-strings (PEP 750), the
> `compression.zstd` module (PEP 784), multiple interpreters in the
> stdlib (PEP 734), officially supported free-threading (PEP 779),
> and `except` without brackets (PEP 758).

### Resources

- Official docs: <https://docs.python.org/3/>
- Language reference: <https://docs.python.org/3/reference/>
- Standard library: <https://docs.python.org/3/library/>
- Tutorial: <https://docs.python.org/3/tutorial/>
- Python Enhancement Proposals: <https://peps.python.org/>
- Style guide (PEP 8): <https://peps.python.org/pep-0008/>
- Package index (PyPI): <https://pypi.org/>
- Downloads: <https://www.python.org/downloads/>
