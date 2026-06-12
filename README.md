# Unified Language Reference

Concise, consistent reference docs for any development language — one file per
language, every file generated from a single master template.

## What's here

- **`TEMPLATE.md`** — the master template and single source of truth. It defines
  every section that *can* appear in a language reference, in fixed order, with a
  directive on when to include or omit each, plus the global authoring rules and
  the canonical paradigm definitions.
- **`languages/`** — one reference per language (`python.md`, `c.md`, `sql.md`, …).
- **`docs/`** — design notes for the template itself.

## How a language doc is produced

1. Walk `TEMPLATE.md` strictly top to bottom.
2. For each section, evaluate its `INCLUDE-IF` / `OMIT-IF` directive against the
   language. Fill the ones that apply; delete the ones that don't — heading and
   all. No empty headings.
3. Fill with the compact prose **feature atom**: `**Name** — purpose`, plus a
   short snippet where it helps.
4. Strip every directive comment. Never reorder. Code stays short and never wraps.

The output is a self-contained reference: a flat list of `##` sections under a
single `#` language title.

## Principles

- **Written for a competent developer** new to the language, not to programming.
  Explain purpose and language-specific semantics, never the basics.
- **Concise but not shallow.** Elegant over exhaustive.
- **Same shape everywhere**, so a reader always knows where to look — and an
  agent can generate new docs consistently.

## Adding a language

Copy `TEMPLATE.md`, follow the four steps above, and save the result to
`languages/<lang>.md`. The three sections that are mandatory for every language:
**Paradigms**, **Standard Library — Start Here**, and **Conventions & Style**.
