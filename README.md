# Unified Language Reference

> One concise, consistently-shaped reference per development language — every
> file generated from a single master template under an enforced contract.

Open any language here and the shape is the same: identity, foundations, logic,
abstraction, runtime, practice, reference — same sections, same order, every
time. Tables carry the data; prose carries the *why*; facts are sourced, not
recalled.

## Languages

| Language | Reference | Paradigms | Typing | Latest target |
|---|---|---|---|---|
| C# | [csharp.md](languages/csharp.md) | multi-paradigm (OO · functional · generic) | static, strong, nullable-aware | C# 14 · .NET 10 (LTS) |
| Python | [python.md](languages/python.md) | multi-paradigm (OO · functional · procedural) | dynamic, strong, duck-typed | Python 3.14 (CPython) |

<!-- Add one row per language, kept alphabetical by the Language column. -->

## How it works

Three artifacts, three jobs:

| Artifact | Job |
|---|---|
| [`TEMPLATE.md`](TEMPLATE.md) | **what to write** — every section that can appear, in fixed order, each with a directive on when to include or omit it and how to fill it |
| [`bootstrap.yaml`](bootstrap.yaml) | **how to read it** — the machine-readable contract: directive grammar, table schemas, mandatory sections, accuracy sources, output invariants |
| [`.claude/skills/language-reference-author/`](.claude/skills/language-reference-author) | **how to run it** — the procedure an agent follows to generate a doc |

Supporting cast: [`.mcp.json`](.mcp.json) wires free, no-auth documentation MCP
servers (Microsoft Learn, DeepWiki) used during research; `languages/` holds the
generated docs; `docs/` holds the design notes.

## Generating a language doc

1. **Research first.** Source every factual table cell — primitive sizes and
   ranges, operators, stdlib modules, tooling, versions — from the official
   spec, official docs, and the wired MCP sources (Microsoft Learn, DeepWiki,
   Context7). Unverifiable values are dropped, never guessed.
2. **Walk the template** top to bottom; for each section evaluate its
   `include_if` / `omit_if` and keep or delete the whole block.
3. **Fill by format** — exact table columns where the data is enumerable, a
   bulleted list of prose atoms (`- **Name** — purpose`, one per bullet) where a
   feature needs a *why*.
4. **Assemble** — `#` title, `##` Contents (a TOC grouped by Part, surviving
   sections only, linked by their full titles), `##` Parts, `###` sections.
   Drop any Part left empty.
5. **Verify** against the contract's `output_invariants`, save to
   `languages/<lang>.md`, and add a row to the table above.

The `language-reference-author` skill performs all five steps.

## Document shape

```
# C#
`badges`

## Contents
- **Identity**
  - Overview
  - Language Type
  - Paradigms
  - Mental Model
- **Foundations**
  - Type System
  - ...
...

## Identity            <- Part (H2)
### Overview           <- Section (H3)
### Language Type
...
```

## Principles

- **Written for a competent developer** new to the language, not to programming.
- **Tables for data, prose for the why** — scannable at a glance.
- **Accurate by sourcing**, not by recall; facts trace to the spec.
- **Same shape everywhere**, so readers and agents always know where to look.

## Adding a language

Run the `language-reference-author` skill (or follow the five steps by hand),
save the result to `languages/<lang>.md`, and add its row to **Languages**.
Mandatory in every doc: **Paradigms**, **Standard Library — Start Here**, and
**Conventions & Style**.
