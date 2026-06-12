# Unified Language Reference — Master Template

> **Single source of content.** An agent fills this top-to-bottom to produce one
> reference per language at `languages/<lang>.md`. The rules for *how* to read
> this file live in [`bootstrap.yaml`](bootstrap.yaml); the step-by-step
> procedure lives in the `language-reference-author` skill. Keep the sections
> whose `include_if` holds, drop the rest, strip every directive comment, and
> never reorder.

---

## How to use this template

1. Load [`bootstrap.yaml`](bootstrap.yaml) — it is the contract. Then walk this
   file strictly **top to bottom**.
2. For each `SECTION`, read its directive and evaluate `include_if` / `omit_if`
   against the target language. Keep it or **delete the whole block** — heading,
   body, and comments. Never leave an empty heading.
3. Fill each kept section by its `format`:
   - `table` — use the columns from the named `table_schema`, exactly.
   - `prose` — use the feature atom: `**Name** — purpose` + an optional snippet.
   - `mixed` — a lead table, then prose for nuance.
4. After all sections are resolved, build the **Contents** block: a TOC grouped
   by Part, listing only the sections that survived, in order.
5. Strip **every** directive comment and Part marker. Replace **every**
   `{{placeholder}}`. Verify the result against `output_invariants`.

## Global rules

- **Audience.** A competent developer new to *this* language, not to
  programming. Explain purpose and language-specific semantics; never teach what
  a variable or loop is.
- **Real estate.** Prefer a **table** whenever the data is enumerable (types,
  operators, data structures, tooling, stdlib, naming). Reserve prose for
  features whose *why* needs a sentence (closures, ownership, paradigms).
- **Code.** Snippets are short, **one statement per line**, and **never wrap** —
  keep every line under ~64 columns. Shorten or split rather than wrap.
- **GFM.** Tables, `> [!NOTE]` / `> [!TIP]` / `> [!WARNING]` callouts for idioms
  and gotchas, and `<details>` for long secondary lists. Header badges optional.
- **Headings.** Title is `#`, Contents and Parts are `##`, sections are `###`.
- **Accuracy.** Source every factual table cell (see `authoring` in the
  contract). If a value is unverifiable, drop the row — never guess.

---

## Paradigm definitions (canonical menu)

Defined once. In a language's **Paradigms** section, copy only the entries that
apply and append a `Here:` clause. Do not reword the canonical definition.

- **Imperative** — a program is a sequence of statements that change state.
- **Procedural** — imperative code organized into reusable procedures.
- **Object-Oriented** — bundles state and behavior into types; objects interact
  through methods.
- **Functional** — computation as the evaluation of functions; values over
  mutation, functions as first-class data.
- **Declarative** — expresses *what* the result should be, not the step-by-step
  *how*.
- **Logic** — facts and rules; an engine derives answers by inference.
- **Generic** — algorithms and types parameterized over the types they act on.
- **Concurrent** — work structured as independently progressing tasks.
- **Event-driven** — control flow directed by events and their handlers.
- **Reactive** — data flows and the propagation of change drive computation.
- **Metaprogramming** — programs that read, generate, or transform code.
- **Array / vectorized** — operations apply to whole arrays at once.
- **Stack-based** — operands and results flow through an implicit stack.
- **Query** — declaratively retrieve and transform sets of data.
- **Markup** — annotate content with structure and semantics via tags.

---

<!-- The blocks below are the template body. Everything from here down is
     walked, filled, and stripped of its comments to produce the output doc. -->

<!-- SECTION: header | (document title)
part: none
include_if: always
format: prose
fill: H1 = language name. Next line = badge chips: paradigm tags, typing
      discipline, compiled/interpreted/transpiled, first-released year.
-->
# {{Language}}

`{{paradigm tags}}` · `{{typing discipline}}` · `{{compiled | interpreted}}` · `{{first released}}`
<!-- /SECTION -->

<!-- TOC
required: true
heading: h2
grouping: by_part
includes: surviving_sections_only
note: Build this AFTER omission. One bullet per Part that retains sections;
      link each surviving section by its anchor, using the section's full
      title as the link text, verbatim. Drop Parts with no sections.
-->
## Contents

- **Identity** — {{links to surviving Identity sections}}
- **Foundations** — {{...}}
- **Logic** — {{...}}
- **Abstraction** — {{...}}
- **Runtime** — {{...}}
- **Practice** — {{...}}
- **Reference** — {{...}}

---

<!-- ===================== PART: identity | Identity ===================== -->
## Identity

<!-- SECTION: overview | Overview
part: identity
include_if: always
format: prose
fill: 2-4 sentences. Primary domains, where it runs, why it exists, what it
      is known for.
-->
### Overview

{{2-4 sentences: domains, runtime/host, reason for being, reputation}}
<!-- /SECTION -->

<!-- SECTION: language-type | Language Type
part: identity
include_if: always
format: table
table_schema: language_type
fill: One row per axis. Keep each cell to a phrase.
-->
### Language Type

| Axis | This language |
|---|---|
| Execution | {{compiled / interpreted / transpiled, to what}} |
| Domain | {{general-purpose / domain-specific: which}} |
| Typing | {{static\|dynamic, strong\|weak, inferred\|manifest, nominal\|structural}} |
| Memory | {{model in a phrase}} |
<!-- /SECTION -->

<!-- SECTION: paradigms | Paradigms
part: identity
include_if: always
format: prose
fill: Copy applicable entries from the canonical menu. Each line:
      **Name** — canonical definition. Here: how it manifests in this language.
-->
### Paradigms

{{applicable paradigm bullets, each ending in a "Here:" clause}}
<!-- /SECTION -->

<!-- SECTION: mental-model | Mental Model
part: identity
include_if: the language has a distinctive philosophy or way of thinking
omit_if: nothing notable beyond its paradigms
format: prose
fill: 2-4 sentences on the core idea a newcomer must internalize.
-->
### Mental Model

{{2-4 sentences on the way of thinking the language rewards}}
<!-- /SECTION -->

<!-- ===================== PART: foundations | Foundations ===================== -->
## Foundations

<!-- SECTION: lexical | Lexical & Syntax
part: foundations
include_if: the language has source syntax
format: prose
fill: comments, statement termination, block style, keyword casing, entry
      point, required file structure. Brief.
-->
### Lexical & Syntax

{{comments, terminators, block style, entry point, file structure}}
<!-- /SECTION -->

<!-- SECTION: variables | Variables & Bindings
part: foundations
include_if: the language has named bindings
omit_if: no concept of variables
format: prose
fill: declaration forms, mutability (const/let/val/var), scope, shadowing,
      constants. One atom per distinct binding form.
-->
### Variables & Bindings

{{declaration forms, mutability, scope, constants}}
<!-- /SECTION -->

<!-- SECTION: type-system | Type System
part: foundations
include_if: the language has types
omit_if: untyped with nothing to say
format: mixed
table_schema: primitives
fill: Lead with the primitives table. Then prose atoms for inference,
      conversion, and nullability. Add a Generics atom only if applicable.
      Source every size/range from the spec.
-->
### Type System

| Syntax | Type | Size | Range | Literal |
|---|---|---|---|---|
| {{i32}} | {{signed integer}} | {{32-bit}} | {{-2^31 .. 2^31-1}} | {{`0i32`}} |
| {{u8}} | {{unsigned integer}} | {{8-bit}} | {{0 .. 255}} | {{`0u8`}} |
| {{f64}} | {{float}} | {{64-bit}} | {{IEEE-754}} | {{`1.0`}} |
| {{bool}} | {{boolean}} | {{1 byte}} | {{true / false}} | {{`true`}} |

**Inference** — {{when and how types are inferred}}
**Conversion** — {{implicit coercion vs explicit cast}}
**Nullability** — {{how absence is represented}}

{{Generics (omit if none): parameterization, bounds, variance}}
<!-- /SECTION -->

<!-- SECTION: data-structures | Data Structures
part: foundations
include_if: the language has composite/aggregate types
omit_if: no composite data
format: table
table_schema: data_structures
fill: One row per built-in structure, with its literal syntax.
-->
### Data Structures

| Structure | Syntax | Ordered | Mutable | Use |
|---|---|---|---|---|
| {{list}} | {{`[1, 2, 3]`}} | {{yes}} | {{yes}} | {{sequence}} |
| {{map}} | {{`{k: v}`}} | {{no}} | {{yes}} | {{lookup}} |
<!-- /SECTION -->

<!-- SECTION: operators | Operators & Expressions
part: foundations
include_if: the language has operators
format: mixed
table_schema: operators
fill: Table grouped by Category (Arithmetic, Comparison, Logical, Bitwise,
      Assignment, Special). Note precedence surprises / overloading in prose
      or a [!NOTE] below.
-->
### Operators & Expressions

| Category | Operator | Name | Example | Note |
|---|---|---|---|---|
| {{Arithmetic}} | {{`+`}} | {{add}} | {{`a + b`}} | {{}} |
| {{Comparison}} | {{`==`}} | {{equal}} | {{`a == b`}} | {{}} |
| {{Special}} | {{`?`}} | {{try}} | {{`f()?`}} | {{propagates error}} |

{{precedence or overloading notes, if any}}
<!-- /SECTION -->

<!-- ===================== PART: logic | Logic ===================== -->
## Logic

<!-- SECTION: control-flow | Control Flow
part: logic
include_if: the language has conditionals or loops
omit_if: purely declarative with no control flow
format: prose
fill: conditionals (if/switch/match), loops (for/while/foreach), pattern
      matching, comprehensions, guards/early return. Atom each.
-->
### Control Flow

{{conditionals, loops, matching, comprehensions, guards}}
<!-- /SECTION -->

<!-- SECTION: functions | Functions
part: logic
include_if: the language has callable abstractions
omit_if: no user-defined callables
format: prose
fill: declaration, parameters (default/named/variadic), return, first-class
      functions, closures, lambdas, higher-order, recursion, generators.
-->
### Functions

{{declaration, parameters, first-class/closures/lambdas, generators}}
<!-- /SECTION -->

<!-- SECTION: error-handling | Error Handling
part: logic
include_if: the language has an error or failure model
omit_if: no error model
format: prose
fill: the mechanism — exceptions (try/catch/finally), result/option types,
      error values, panics — plus propagation and assertions.
-->
### Error Handling

{{mechanism, propagation, assertions}}
<!-- /SECTION -->

<!-- ===================== PART: abstraction | Abstraction ===================== -->
## Abstraction

<!-- SECTION: object-model | Object Model
part: abstraction
include_if: the language has user-defined types with behavior
omit_if: purely procedural (e.g. C) or non-programming (SQL, CSS)
format: prose
fill: type definition, instantiation, fields/methods, encapsulation/visibility,
      inheritance, interfaces/traits/protocols, polymorphism, composition.
-->
### Object Model

{{definition, instantiation, encapsulation, inheritance, interfaces, polymorphism}}
<!-- /SECTION -->

<!-- SECTION: functional-constructs | Functional Constructs
part: abstraction
include_if: the language meaningfully supports functional style
omit_if: no functional features beyond plain functions
format: prose
fill: immutability, pure functions, algebraic data types, option/result,
      pattern matching on data, currying/partial application, lazy evaluation.
-->
### Functional Constructs

{{immutability, ADTs, option/result, currying, laziness}}
<!-- /SECTION -->

<!-- SECTION: modules | Modules & Namespaces
part: abstraction
include_if: the language has modular code units or namespacing
omit_if: single global scope only
format: prose
fill: module/namespace unit, import/export, cross-module visibility,
      packaging, and the dominant package manager/registry.
-->
### Modules & Namespaces

{{module unit, import/export, visibility, packaging/registry}}
<!-- /SECTION -->

<!-- SECTION: metaprogramming | Metaprogramming & Reflection
part: abstraction
include_if: the language can inspect or generate code
omit_if: none
format: prose
fill: macros, reflection, decorators/attributes/annotations, code generation,
      eval, compile-time evaluation. Note cost/safety tradeoffs briefly.
-->
### Metaprogramming & Reflection

{{macros, reflection, decorators, codegen, compile-time eval}}
<!-- /SECTION -->

<!-- ===================== PART: runtime | Runtime ===================== -->
## Runtime

<!-- SECTION: memory | Memory Management
part: runtime
include_if: the language exposes a notable memory model
omit_if: managed and unremarkable, or not applicable
format: prose
fill: allocation model (manual / GC / ARC / ownership-borrow), pointers and
      references, stack vs heap, lifetimes, destructors/RAII, weak references.
-->
### Memory Management

{{allocation model, references/pointers, lifetimes, destruction}}
<!-- /SECTION -->

<!-- SECTION: concurrency | Concurrency & Parallelism
part: runtime
include_if: the language has concurrency primitives
omit_if: strictly single-threaded with no async model
format: prose
fill: threads, async/await, futures/promises, channels/goroutines, actors,
      locks/atomics, the event loop. Atom each with the idiomatic form.
-->
### Concurrency & Parallelism

{{threads, async, channels/actors, synchronization}}
<!-- /SECTION -->

<!-- ===================== PART: practice | Practice ===================== -->
## Practice

<!-- SECTION: standard-library | Standard Library — Start Here
part: practice
include_if: always
format: mixed
table_schema: standard_library
fill: One line on where the stdlib lives and how to import. Then a table of the
      8-15 modules a developer reaches for first. Purpose <= 8 words.
-->
### Standard Library — Start Here

{{one line: where the stdlib lives and how to access it}}

| Module | Purpose | Import |
|---|---|---|
| {{name}} | {{<= 8-word purpose}} | {{`import x`}} |
| {{name}} | {{<= 8-word purpose}} | {{`import y`}} |
<!-- /SECTION -->

<!-- SECTION: tooling | Tooling & Ecosystem
part: practice
include_if: always
format: table
table_schema: tooling
fill: One row per tool: runtime/compiler, build, package manager, formatter,
      linter, test runner, REPL, debugger, dominant framework(s).
-->
### Tooling & Ecosystem

| Tool | Role | Invoke |
|---|---|---|
| {{cargo}} | {{build + package manager}} | {{`cargo build`}} |
| {{clippy}} | {{linter}} | {{`cargo clippy`}} |
<!-- /SECTION -->

<!-- SECTION: conventions | Conventions & Style
part: practice
include_if: always
format: mixed
table_schema: naming
fill: Naming table per identifier kind. Then prose for indentation, project
      layout, and doc-comment style. End with a link to the official style guide.
-->
### Conventions & Style

| Identifier | Convention | Example |
|---|---|---|
| {{variables}} | {{snake_case}} | {{`user_id`}} |
| {{types}} | {{PascalCase}} | {{`HttpClient`}} |
| {{constants}} | {{UPPER_SNAKE}} | {{`MAX_LEN`}} |
| {{files}} | {{convention}} | {{`user_service.ext`}} |

{{indentation/formatting, project layout, doc-comment style}}

{{official style guide: link}}
<!-- /SECTION -->

<!-- SECTION: idioms | Idioms & Gotchas
part: practice
include_if: the language has notable idioms or common footguns
omit_if: nothing worth flagging
format: prose
fill: short list. Each: the idiom or the gotcha in one or two sentences.
      Use > [!WARNING] for the sharp edges.
-->
### Idioms & Gotchas

{{idiomatic patterns and common pitfalls}}
<!-- /SECTION -->

<!-- ===================== PART: reference | Reference ===================== -->
## Reference

<!-- SECTION: versioning | Versioning & Editions
part: reference
include_if: version or edition differences materially affect usage
omit_if: not relevant
format: prose
fill: how versioning works, notable edition/version differences, major
      deprecations. Brief.
-->
### Versioning & Editions

{{versioning scheme, notable differences, deprecations}}
<!-- /SECTION -->

<!-- SECTION: resources | Resources
part: reference
include_if: always
format: prose
fill: official docs, the language spec, the style guide, the package registry,
      and one or two high-quality learning resources. Links only.
-->
### Resources

- {{official docs}}
- {{language spec}}
- {{style guide}}
- {{package registry}}
<!-- /SECTION -->
