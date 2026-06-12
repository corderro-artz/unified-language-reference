# Worked example — expected output shape

A correctly filled fragment of `languages/rust.md`, showing the document head,
a grouped TOC, a Part with mixed (table + prose) and prose sections, and the
formatting rules in practice. Use it as the visual target. Facts here are
illustrative — always source them in Phase 1.

---

```markdown
# Rust

`multi-paradigm` · `static, strong, inferred` · `compiled` · `2015`

## Contents

- **Identity**
  - [Overview](#overview)
  - [Language Type](#language-type)
  - [Paradigms](#paradigms)
- **Foundations**
  - [Type System](#type-system)
  - [Data Structures](#data-structures)
  - [Operators & Expressions](#operators--expressions)
- **Runtime**
  - [Memory Management](#memory-management)

---

## Identity

### Overview

Systems language focused on memory safety without a garbage collector. Compiles
to native binaries; common in CLIs, embedded, WebAssembly, and infrastructure.

### Language Type

| Axis | This language |
|---|---|
| Execution | compiled to native via LLVM |
| Domain | general-purpose, systems |
| Typing | static, strong, inferred, nominal |
| Memory | ownership + borrowing, no GC |

### Paradigms

- **Imperative** — a program is a sequence of statements that change state. Here: the default statement flow.
- **Functional** — computation as the evaluation of functions; values over mutation. Here: iterators, closures, `Option`/`Result`, pattern matching.
- **Generic** — algorithms and types parameterized over the types they act on. Here: generics with trait bounds.

## Foundations

### Type System

| Syntax | Type | Size | Range | Default | Literal |
|---|---|---|---|---|---|
| i32 | signed integer | 32-bit | -2^31 .. 2^31-1 | `0` | `0i32` |
| u8 | unsigned integer | 8-bit | 0 .. 255 | `0` | `0u8` |
| f64 | float | 64-bit | IEEE-754 | `0.0` | `1.0` |
| bool | boolean | 1 byte | true / false | `false` | `true` |

- **Inference** — `let x = 5` binds `i32` by default; annotate to widen.
- **Conversion** — no implicit numeric coercion; cast with `as`.
- **Nullability** — no null; absence is `Option<T>`.

> [!WARNING]
> Arithmetic overflow panics in debug, wraps in release. Use `checked_add`.

### Data Structures

| Structure | Syntax | Ordered | Mutable | Use |
|---|---|---|---|---|
| Vec | `vec![1, 2, 3]` | yes | yes | growable sequence |
| HashMap | `HashMap::new()` | no | yes | key lookup |
| tuple | `(1, "a")` | yes | no | fixed group |

### Operators & Expressions

| Category | Operator | Name | Example | Note |
|---|---|---|---|---|
| Arithmetic | `+` | add | `a + b` | overloadable via `Add` |
| Comparison | `==` | equal | `a == b` | needs `PartialEq` |
| Special | `?` | try | `f()?` | propagates `Err`/`None` |

## Runtime

### Memory Management

- **Ownership** — each value has one owner; dropping the owner frees the value.
- **Borrowing** — references are shared (`&`) or exclusive (`&mut`), never both.
- **Lifetimes** — the compiler checks references never outlive their data.

> [!NOTE]
> No garbage collector; freeing is deterministic at scope exit (RAII).
```

---

## What this demonstrates

- **Header** above the Parts; badges on one line.
- **Contents** as nested bullets (bold Part bullet + indented section
  sub-bullets), only surviving sections (Logic, Abstraction, Practice, Reference
  were omitted here for brevity — a real doc keeps the mandatory ones).
- **Table vs prose:** Type System is `mixed` (primitives table + a bulleted atom
  list for inference/conversion/nullability); Memory Management is a `prose`
  atom list. Atoms are always list items, never bare consecutive lines.
- **Exact schema columns** for each table, including the primitives **Default**
  column (the zeroed/default value of each type).
- **Code never wraps;** snippets are inline and short.
- **Examples** on primary code sections — a short snippet inline, longer ones in
  a collapsed `<details><summary>Example</summary>` (see `languages/csharp.md`
  for live examples; they're omitted here only to keep this sample inside one
  fenced block).
- **Callouts** carry the gotchas.
- No placeholders, no directive comments, no empty headings.
