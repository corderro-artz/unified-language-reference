# Unified Language Reference — Master Template Design

**Date:** 2026-06-11
**Status:** Approved. Master template implemented in `TEMPLATE.md`.

## Problem

Build a GitHub repo holding concise reference docs for any development language —
one file per language. Each doc must iterate the language's features and define
their *purpose* elegantly: not insultingly simple, but readable by most
developers. A master template must define every section and its layout so an
agent can generate per-language docs easily and consistently, pulling in the
sections that apply (top to bottom) and omitting those that don't.

## Requirements (from the brief)

1. The template considers, up front, every component that *could* exist in any
   language, so each doc can either outline a feature or omit it cleanly. Some
   languages have fewer features and a different thought process (e.g. C has no
   objects).
2. **Standardized conventions** are their own mandatory section.
3. A section captures the **most commonly used namespaces / built-ins** as an
   at-a-glance starting point.
4. **Paradigms** are explained: the paradigm and language type per language. The
   *definition* of a paradigm is shared (define a word once), but the correct
   paradigms and concepts are stated per language.
5. The format lets an agent generate a language-specific doc easily and
   consistently — pull required sections top to bottom, omit what's inapplicable.

## Decisions

| # | Question | Decision |
|---|---|---|
| Q1 | Language scope | **Everything dev-facing** — general-purpose + scripting + markup/style + query + config + regex. Widest superset; heavy section-level omission. |
| Q2 | Template mechanism | **Annotated single master.** One `TEMPLATE.md`, fixed top-to-bottom order, each section wrapped in directive comments (include/omit/fill). Agent walks it, fills or deletes each section, strips comments. |
| Q3 | Feature atom | **Compact prose entry:** `**Name** — 1–2 sentence purpose` + optional short snippet. Tables only for genuine at-a-glance lists (built-ins, naming). **Hard rule: code never wraps** — short, one statement per line, under ~64 columns. |
| Q4 | Paradigms / reuse | **The master holds all canonical definitions; a language doc pulls the applicable one(s)** and appends a per-language "Here:" manifestation. Generalizes: the master is the single source of all reusable text; output docs are self-contained. |

## Approach

Single-source **annotated master** → self-contained **generated docs**. The
master is both the structural skeleton (ordered, directive-wrapped sections) and
the repository of canonical reusable text (paradigm menu, global rules). A
language doc is produced by walking the master, evaluating each section's
`INCLUDE-IF` / `OMIT-IF`, filling the survivors, and stripping all scaffolding.

Authoring rules live in the `TEMPLATE.md` preamble (one file) rather than a
separate guide.

## Repo structure

```
unified-language-reference/
  README.md          index + how to read
  TEMPLATE.md        master: rules + paradigm menu + ordered section superset
  docs/              design notes (this file)
  languages/         one reference per language (python.md, c.md, sql.md, ...)
```

## Section superset (ordered, grouped in Parts)

Each language pulls the applicable subset, top to bottom. `*` = near-always.

**A — Identity:** Header\* · Overview\* · Language Type\* · Paradigms\* · Mental Model

**B — Foundations:** Lexical & Syntax · Variables & Bindings · Type System
(generics as a subsection) · Data Structures · Operators & Expressions

**C — Logic:** Control Flow · Functions · Error Handling

**D — Abstraction:** Object Model · Functional Constructs · Modules & Namespaces ·
Metaprogramming & Reflection

**E — Runtime:** Memory Management · Concurrency & Parallelism

**F — Practice:** Standard Library — Start Here\* (table) · Tooling & Ecosystem\* ·
Conventions & Style\* · Idioms & Gotchas

**G — Reference:** Versioning & Editions · Resources\*

Boundary rule that prevents overlap: **Functions** covers callable *mechanics*
(closures, lambdas, higher-order, generators); **Functional Constructs** covers
paradigm-level *values* (immutability, ADTs, option/result, currying, laziness).

The three mandatory requirements map to: **Conventions & Style** (own section,
always), **Standard Library — Start Here** (own table, always), and
**Paradigms** (always, master-defined).

## Global authoring rules

- Audience: a competent developer new to the language. Explain purpose and
  semantics, not the basics. Peer tone, elegant over exhaustive, never a tutorial.
- Feature atom as in Q3; tables only where at-a-glance fits.
- Code never wraps: short, one statement per line, under ~64 columns.
- Omission is normal and happens at the whole-section level — delete the block,
  never leave an empty heading.
- Never reorder. Replace every placeholder. Strip every directive comment.

## Paradigms handling

The master's **Paradigm definitions (canonical menu)** lists each paradigm once
(Imperative, Procedural, Object-Oriented, Functional, Declarative, Logic,
Generic, Concurrent, Event-driven, Reactive, Metaprogramming, Array, Stack-based,
Query, Markup). A language's Paradigms section copies the applicable entries
verbatim and appends a `Here:` clause describing the manifestation.

## Directive schema (per section)

```
<!-- SECTION: id | Title
INCLUDE-IF: <condition>
OMIT-IF: <condition>
PURPOSE: <what the section captures>
FILL: <atom rule, depth, what to enumerate>
-->
## Title
{{placeholder}}
<!-- /SECTION -->
```

## Example (wrap-free)

Python → **Functions** (included):

```
**Closures** — a function keeps access to its
defining scope, carrying state without a class.
def counter():
    n = 0
    def inc(): nonlocal n; n += 1; return n
    return inc
```

C → **Object Model** is omitted entirely (procedural; `INCLUDE-IF` fails) with no
empty heading left behind.

## Repo name

`unified-language-reference` (chosen). Alternatives considered: `polyglot-codex`,
`language-codex`, `langref`, `dev-lang-reference`.
