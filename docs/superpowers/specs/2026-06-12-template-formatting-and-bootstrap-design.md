# Template Formatting, Bootstrap Contract & Authoring Skill — Design

**Date:** 2026-06-12
**Status:** Approved. Implemented in `TEMPLATE.md`, `bootstrap.yaml`,
`README.md`, and `.claude/skills/language-reference-author/`.
**Builds on:** `2026-06-11-unified-language-reference-design.md`.

## Problem

The first master template under-used the page: only two sections were tables,
everything else was prose, and tabular data (primitive types, operators) was
forced into sentences. We also need a *required, auto-derived* table of contents,
and a decision on whether to add a machine-readable specification layer.

## Decisions

| # | Question | Decision |
|---|---|---|
| Q1 | Machine-readable layer | **One source + meta-bootstrap.** `TEMPLATE.md` stays the sole content source; `bootstrap.yaml` is a contract describing *how to interpret* the template (grammar, invariants), not *what* sections say. No content duplication → no drift. |
| Q2 | Markdown palette | **GitHub-flavored, rich.** Tables, `<details>` for long tails, `> [!NOTE]/[!WARNING]` callouts, anchor-link TOC, optional header badges. The repo lives on GitHub. |
| Q3 | Doc structure + TOC | **Grouped body + TOC.** Parts render as `##`, sections as `###`; the required Contents block groups surviving sections under their Part. |

### On the machine-readable spec (the explicit question)

A *full parallel manifest* (every section, column, and rule mirrored in YAML)
would be a second source of truth that drifts from the markdown — "another piece
of the puzzle" — and only earns its keep if non-LLM tooling (a linter, a
generator, a site) consumes it. The enforcement the user actually wants —
"a bootstrap that enforces how to interpret" — is obtained far more cheaply by a
**meta-contract** that describes structure and invariants only. That is
`bootstrap.yaml`. Chosen accordingly.

## Architecture — three artifacts

| Artifact | Job | Drift risk |
|---|---|---|
| `TEMPLATE.md` | content + per-section directives (what to write) | — |
| `bootstrap.yaml` | grammar, format rules, table schemas, invariants (how to read) | none — holds no content |
| `language-reference-author` skill | the procedure (how to run it) | none — references the above |

## Formatting model

**Rule:** enumerable/tabular data → **table**; a feature that needs a *why* →
**prose atom** (`**Name** — purpose` + short snippet); both → **mixed** (lead
table, prose below).

**Table schemas (fixed columns, defined in `bootstrap.yaml`):**

| Schema | Columns |
|---|---|
| `primitives` | Syntax · Type · Size · Range · Literal |
| `data_structures` | Structure · Syntax · Ordered · Mutable · Use |
| `operators` | Category · Operator · Name · Example · Note |
| `tooling` | Tool · Role · Invoke |
| `standard_library` | Module · Purpose · Import |
| `naming` | Identifier · Convention · Example |
| `language_type` | Axis · This language |

Sections that became tables/mixed: Language Type, Type System, Data Structures,
Operators, Standard Library, Tooling, Conventions. Sections that stay prose
(the *why* matters): Overview, Paradigms, Mental Model, Lexical, Variables,
Control Flow, Functions, Error Handling, Object Model, Functional Constructs,
Modules, Metaprogramming, Memory, Concurrency, Idioms, Versioning, Resources.

**GFM:** `<details>` for long secondary enumerations; `> [!WARNING]` /
`> [!NOTE]` for gotchas and idioms; header badges optional. Code stays short,
one statement per line, never wraps, ≤ 64 columns.

## Document skeleton

```
# {{Language}}                 (H1, from header section)
`badges`
## Contents                    (H2, required, grouped by Part, survivors only)
---
## Identity                    (H2 Part)
### Overview                   (H3 section)
### Language Type
...
## Foundations
### Type System
...
```

The header (title + badges) sits above the Parts. The TOC is built **after**
omission and lists only surviving sections, grouped by Part; a Part with no
surviving sections is dropped from both body and TOC.

## Required TOC rule

Defined in `bootstrap.yaml` under `structure.toc` and enforced by
`output_invariants.toc_matches`: the Contents block must contain exactly the
surviving sections, in template order, grouped by Part, each linking to a valid
heading anchor.

## Authoring skill

`.claude/skills/language-reference-author/` operationalizes the contract in five
phases: **Research** (source facts from spec/docs/Context7 — never guess) →
**Walk & fill** (top-to-bottom, table-or-prose per directive) → **Assemble**
(headings, grouped TOC, strip scaffolding) → **Verify** (the `output_invariants`
checklist) → **Write** (`languages/<lang>.md`). Curated for consistency (no
reordering, fixed columns, mandatory sections) and accuracy (sourced facts,
unverifiable rows omitted). A `reference/example-section.md` anchors the expected
output shape.

## Accuracy sourcing

`bootstrap.yaml.authoring` fixes the source priority: official specification →
official docs → Context7 → reputable secondary (corroboration only). Every
factual table cell must trace to one of these; conflicts resolve toward the
official spec; unverifiable values are dropped, not guessed. Context7 belongs to
the research phase, not the template format.

The repo ships a project `.mcp.json` wiring two free, no-auth documentation MCP
servers as concrete access points to those sources: **Microsoft Learn MCP**
(`https://learn.microsoft.com/api/mcp`) for the Microsoft stack, and **DeepWiki
MCP** (`https://mcp.deepwiki.com/mcp`) for reading official spec/reference repos
on GitHub. `bootstrap.yaml.authoring.mcp_sources` records what each is best for.
Freemium or paid doc services are intentionally excluded.

## Out of scope

Building a standalone linter/generator/site that consumes a full YAML manifest.
Reconsider only if programmatic validation or rendering becomes a requirement.
