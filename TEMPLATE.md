# Unified Language Reference — Master Template

> **This is the single source of truth.** An agent (or a human) fills this
> document top-to-bottom to produce one reference file per language at
> `languages/<lang>.md`. Keep the sections whose `INCLUDE-IF` holds, delete the
> rest, strip every directive comment, and never reorder. The result is a
> self-contained reference for one language.

---

## How to use this template

1. Copy the section order below. Walk it strictly **top to bottom**.
2. For each `SECTION`, read its directive comment and evaluate `INCLUDE-IF`
   against the target language. If it holds, fill the section. If `OMIT-IF`
   holds (or `INCLUDE-IF` fails), **delete the entire block** — heading, body,
   and comments. Never leave an empty heading behind.
3. Fill included sections using the **feature atom** (see rules) and replace
   every `{{placeholder}}`. No placeholder may survive into the output.
4. Strip **all** directive comments (`<!-- ... -->`) and the `PART` dividers
   from the final language doc.
5. Output is a flat list of `##` sections under a single `#` title.

---

## Global authoring rules

- **Audience.** A competent developer who is new to *this* language, not to
  programming. Explain purpose and language-specific semantics. Do not define
  what a variable, loop, or function is in the abstract.
- **Tone.** Peer to peer. Concise, precise, a little opinionated where the
  community is. Elegant over exhaustive. Never a tutorial.
- **Feature atom (default).** Document each feature as:

  ```
  **Name** — one or two sentences on its purpose (the why), not its mechanics.
  short snippet, optional
  ```

  Use a **table** only where an at-a-glance list genuinely reads better
  (built-ins overview, naming conventions). Nowhere else.
- **Code.** Snippets are short and illustrative, **one statement per line**,
  and **must never wrap** — keep every line under ~64 columns. If a snippet
  would wrap, shorten it or split it across short lines. Never rely on
  horizontal scrolling. No multi-screen examples.
- **Omission is normal.** Most languages use a subset of these sections. A
  procedural language deletes Object Model. A config language keeps only
  Identity, a little of Foundations, Practice, and Reference. That is expected.
- **Consistency.** Identical section titles, order, and atom shape across every
  language, so a reader always knows where to look.

---

## Paradigm definitions (canonical menu)

Defined once here. In a language's **Paradigms** section, copy only the entries
that apply and append a `Here:` clause describing how that paradigm shows up in
the language. Do not reword the canonical definition.

- **Imperative** — a program is a sequence of statements that change state.
- **Procedural** — imperative code organized into reusable procedures.
- **Object-Oriented** — bundles state and behavior into types; objects
  interact through methods.
- **Functional** — computation as evaluation of functions; values over
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

<!-- =================== PART A — IDENTITY =================== -->

<!-- SECTION: header | Header
INCLUDE-IF: always
PURPOSE: title, tagline, and one-line classification badges
FILL: H1 = language name. Italic tagline. Badge line of paradigm tags,
      typing discipline, compiled/interpreted/transpiled, first-released year.
-->
# {{Language}}

*{{one-line tagline — what it is in a breath}}*

`{{paradigm tags}}` · `{{typing discipline}}` · `{{compiled | interpreted}}` · `{{first released}}`
<!-- /SECTION -->

<!-- SECTION: overview | Overview
INCLUDE-IF: always
PURPOSE: what the language is and what it is for
FILL: 2-4 sentences. Primary domains, where it runs, why it exists,
      what it is known for.
-->
## Overview

{{2-4 sentences: domains, runtime/host, reason for being, reputation}}
<!-- /SECTION -->

<!-- SECTION: language-type | Language Type
INCLUDE-IF: always
PURPOSE: classify the language along the standard axes
FILL: compiled/interpreted/transpiled; general-purpose vs domain-specific;
      typing discipline (static/dynamic, strong/weak, manifest/inferred,
      nominal/structural); execution/memory model in one line each.
-->
## Language Type

- **Execution** — {{compiled / interpreted / transpiled, to what}}
- **Domain** — {{general-purpose / domain-specific: which}}
- **Typing** — {{static|dynamic, strong|weak, manifest|inferred, nominal|structural}}
- **Model** — {{execution + memory model in one line}}
<!-- /SECTION -->

<!-- SECTION: paradigms | Paradigms
INCLUDE-IF: always
PURPOSE: which paradigms the language supports and how
FILL: Copy applicable entries from the canonical menu. Each line:
      **Name** — canonical definition. Here: how it manifests in this language.
-->
## Paradigms

{{applicable paradigm bullets, each ending with a "Here:" clause}}
<!-- /SECTION -->

<!-- SECTION: mental-model | Mental Model
INCLUDE-IF: the language has a distinctive philosophy or way of thinking
OMIT-IF: nothing notable beyond its paradigms
PURPOSE: the mindset the language rewards
FILL: 2-4 sentences. What it optimizes for, the core idea a newcomer must
      internalize (e.g. ownership, "everything is an object", data-oriented).
-->
## Mental Model

{{2-4 sentences on the way of thinking the language rewards}}
<!-- /SECTION -->

<!-- =================== PART B — FOUNDATIONS =================== -->

<!-- SECTION: lexical | Lexical & Syntax
INCLUDE-IF: the language has source syntax
OMIT-IF: not applicable
PURPOSE: the visible skeleton of a source file
FILL: comments, statement termination, blocks/indentation, casing of
      keywords, entry point, and any required file structure. Brief.
-->
## Lexical & Syntax

{{comments, terminators, block style, entry point, file structure}}
<!-- /SECTION -->

<!-- SECTION: variables | Variables & Bindings
INCLUDE-IF: the language has named bindings
OMIT-IF: no concept of variables
PURPOSE: how names are bound to values and whether they can change
FILL: declaration forms, mutability (const/let/val/var), scope, shadowing,
      constants. One atom per distinct binding form.
-->
## Variables & Bindings

{{declaration forms, mutability, scope, constants}}
<!-- /SECTION -->

<!-- SECTION: types | Type System
INCLUDE-IF: the language has types
OMIT-IF: untyped / stringly-typed with nothing to say
PURPOSE: the primitive and user-facing type machinery
FILL: primitives, declaration/annotation, inference, conversion/coercion,
      nullability. Include a Generics subsection if the language has them.
-->
## Type System

{{primitives, annotation, inference, conversion, nullability}}

{{Generics — only if applicable: parameterization, constraints, variance}}
<!-- /SECTION -->

<!-- SECTION: data-structures | Data Structures
INCLUDE-IF: the language has composite/aggregate types
OMIT-IF: no composite data
PURPOSE: the built-in ways to group data
FILL: arrays/lists, maps/dicts, sets, tuples, structs/records, enums.
      One atom each, with the literal syntax.
-->
## Data Structures

{{lists, maps, sets, tuples, records, enums — atom each}}
<!-- /SECTION -->

<!-- SECTION: operators | Operators & Expressions
INCLUDE-IF: the language has operators
OMIT-IF: not applicable
PURPOSE: how values are combined into expressions
FILL: arithmetic, comparison, logical, bitwise, assignment, and any special
      operators (null-coalescing, spread, pipe, ternary, range). Note
      precedence surprises and operator overloading if present.
-->
## Operators & Expressions

{{operator families + special operators + precedence/overloading notes}}
<!-- /SECTION -->

<!-- =================== PART C — LOGIC =================== -->

<!-- SECTION: control-flow | Control Flow
INCLUDE-IF: the language has conditionals or loops
OMIT-IF: purely declarative with no control flow
PURPOSE: how execution branches and repeats
FILL: conditionals (if/switch/match), loops (for/while/foreach),
      pattern matching, comprehensions, guards/early return. Atom each.
-->
## Control Flow

{{conditionals, loops, matching, comprehensions, guards}}
<!-- /SECTION -->

<!-- SECTION: functions | Functions
INCLUDE-IF: the language has callable abstractions
OMIT-IF: no user-defined callables
PURPOSE: how reusable behavior is defined and passed around
FILL: declaration, parameters (default/named/variadic), return, first-class
      functions, closures, lambdas, higher-order, recursion, generators.
-->
## Functions

{{declaration, parameters, first-class/closures/lambdas, generators}}
<!-- /SECTION -->

<!-- SECTION: error-handling | Error Handling
INCLUDE-IF: the language has an error or failure model
OMIT-IF: no error model
PURPOSE: how failure is represented and propagated
FILL: the mechanism — exceptions (try/catch/finally), result/option types,
      error values, panics — plus propagation and assertions.
-->
## Error Handling

{{mechanism, propagation, assertions}}
<!-- /SECTION -->

<!-- =================== PART D — ABSTRACTION =================== -->

<!-- SECTION: object-model | Object Model
INCLUDE-IF: the language has user-defined types with behavior
OMIT-IF: purely procedural (e.g. C) or non-programming (SQL, CSS)
PURPOSE: how the language models objects and their relationships
FILL: type definition, instantiation, fields/methods, encapsulation/visibility,
      inheritance, interfaces/traits/protocols, polymorphism, composition.
-->
## Object Model

{{definition, instantiation, encapsulation, inheritance, interfaces, polymorphism}}
<!-- /SECTION -->

<!-- SECTION: functional-constructs | Functional Constructs
INCLUDE-IF: the language meaningfully supports functional style
OMIT-IF: no functional features beyond plain functions
PURPOSE: the functional-paradigm machinery beyond basic functions
FILL: immutability, pure functions, algebraic data types, option/result,
      pattern matching on data, currying/partial application, lazy evaluation.
-->
## Functional Constructs

{{immutability, ADTs, option/result, currying, laziness}}
<!-- /SECTION -->

<!-- SECTION: modules | Modules & Namespaces
INCLUDE-IF: the language has modular code units or namespacing
OMIT-IF: single global scope only
PURPOSE: how code is partitioned, named, and shared
FILL: module/namespace unit, import/export, cross-module visibility,
      packaging, and the dominant package manager/registry.
-->
## Modules & Namespaces

{{module unit, import/export, visibility, packaging/registry}}
<!-- /SECTION -->

<!-- SECTION: metaprogramming | Metaprogramming & Reflection
INCLUDE-IF: the language can inspect or generate code
OMIT-IF: none
PURPOSE: the language's reflective and generative capabilities
FILL: macros, reflection, decorators/attributes/annotations, code generation,
      eval, compile-time evaluation. Note the cost/safety tradeoffs briefly.
-->
## Metaprogramming & Reflection

{{macros, reflection, decorators, codegen, compile-time eval}}
<!-- /SECTION -->

<!-- =================== PART E — RUNTIME =================== -->

<!-- SECTION: memory | Memory Management
INCLUDE-IF: the language exposes a notable memory model
OMIT-IF: managed and unremarkable, or not applicable
PURPOSE: how memory is allocated and reclaimed
FILL: allocation model (manual / GC / ARC / ownership-borrow), pointers and
      references, stack vs heap, lifetimes, destructors/RAII, weak references.
-->
## Memory Management

{{allocation model, references/pointers, lifetimes, destruction}}
<!-- /SECTION -->

<!-- SECTION: concurrency | Concurrency & Parallelism
INCLUDE-IF: the language has concurrency primitives
OMIT-IF: strictly single-threaded with no async model
PURPOSE: how concurrent and parallel work is expressed
FILL: threads, async/await, futures/promises, channels/goroutines, actors,
      locks/atomics, the event loop. Atom each with the idiomatic form.
-->
## Concurrency & Parallelism

{{threads, async, channels/actors, synchronization}}
<!-- /SECTION -->

<!-- =================== PART F — PRACTICE =================== -->

<!-- SECTION: stdlib | Standard Library — Start Here
INCLUDE-IF: always
PURPOSE: the most-used built-ins/namespaces, at a glance, as a starting point
FILL: a TABLE of the modules/namespaces a developer reaches for first.
      8-15 rows. Purpose column <= 8 words. Note where the stdlib lives and
      how it is accessed in one line above the table.
-->
## Standard Library — Start Here

{{one line: where the stdlib lives and how to access it}}

| Module / Namespace | Purpose |
|---|---|
| {{name}} | {{<= 8-word purpose}} |
| {{name}} | {{<= 8-word purpose}} |
<!-- /SECTION -->

<!-- SECTION: tooling | Tooling & Ecosystem
INCLUDE-IF: always
PURPOSE: the everyday toolchain
FILL: runtime/compiler, build tool, package manager, formatter, linter, test
      framework, REPL, debugger, and the dominant frameworks. Keep it brief.
-->
## Tooling & Ecosystem

{{runtime, build, package manager, formatter, linter, test, REPL, frameworks}}
<!-- /SECTION -->

<!-- SECTION: conventions | Conventions & Style
INCLUDE-IF: always
PURPOSE: the standardized conventions the community follows
FILL: naming per identifier kind (as a small table), indentation/formatting,
      file and project layout, documentation-comment style, and a link to the
      official style guide. This section is mandatory for every language.
-->
## Conventions & Style

| Identifier | Convention |
|---|---|
| {{variables}} | {{e.g. snake_case}} |
| {{types}} | {{e.g. PascalCase}} |
| {{constants}} | {{e.g. UPPER_SNAKE}} |
| {{files}} | {{convention}} |

{{indentation/formatting, project layout, doc-comment style, official guide link}}
<!-- /SECTION -->

<!-- SECTION: idioms | Idioms & Gotchas
INCLUDE-IF: the language has notable idioms or common footguns
OMIT-IF: nothing worth flagging
PURPOSE: the idiomatic "right way" and the traps newcomers hit
FILL: a short list. Each: the idiom or the gotcha in one or two sentences.
-->
## Idioms & Gotchas

{{idiomatic patterns and common pitfalls}}
<!-- /SECTION -->

<!-- =================== PART G — REFERENCE =================== -->

<!-- SECTION: versioning | Versioning & Editions
INCLUDE-IF: version or edition differences materially affect usage
OMIT-IF: not relevant
PURPOSE: what a reader must know about versions
FILL: how versioning works, notable edition/version differences, and major
      deprecations. Brief.
-->
## Versioning & Editions

{{versioning scheme, notable differences, deprecations}}
<!-- /SECTION -->

<!-- SECTION: resources | Resources
INCLUDE-IF: always
PURPOSE: where to go next
FILL: official docs, the language spec, the style guide, the package registry,
      and one or two high-quality learning resources. Links only.
-->
## Resources

- {{official docs}}
- {{language spec}}
- {{style guide}}
- {{package registry}}
<!-- /SECTION -->
