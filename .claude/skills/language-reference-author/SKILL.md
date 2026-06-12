---
name: language-reference-author
description: Use when creating or updating a per-language reference doc (languages/<lang>.md) in the unified-language-reference repo from TEMPLATE.md. Produces one consistent, accurate reference by following bootstrap.yaml — research authoritative facts first (Context7 + official spec/docs), walk sections top-to-bottom, fill tables or prose per directive, build the grouped Table of Contents, and verify against the output invariants before writing. Trigger when the user asks to "add a language", "write the <X> reference", "generate languages/<x>.md", or document a language from the master template.
---

# Language Reference Author

Generate `languages/<lang>.md` from the master template with the same shape and
sourced-not-guessed accuracy every time. This skill is the procedure; the
template is the content; `bootstrap.yaml` is the contract. The contract wins any
disagreement.

## Load first (every run)

1. `TEMPLATE.md` — section content + per-section directives.
2. `bootstrap.yaml` — grammar, format rules, `table_schemas`,
   `mandatory_sections`, `authoring`, `output_invariants`.

Do not start writing until both are loaded and the target language and version
are fixed (use the latest stable release unless told otherwise).

## The five phases

Work them in order. Do not skip Phase 1. Do not finish before Phase 4 passes.

### Phase 1 — Research (accuracy before a single line)

Gather authoritative facts for every section whose data is factual, listed in
`bootstrap.yaml.authoring.facts_required_for`: **primitives** (syntax, sizes,
ranges, literals), **operators**, **standard_library** modules, **tooling**
commands, and **versioning**.

Sources, in priority order (from the contract):

1. Official language specification / reference.
2. Official documentation.
3. **Context7** — `resolve-library-id` then `query-docs` for stdlib, framework,
   and library facts.
4. Reputable secondary source — only to corroborate the above.

**Wired MCP sources** (configured in `.mcp.json`, all free and no-auth): prefer
**Microsoft Learn MCP** for the Microsoft stack (C#, F#, .NET, PowerShell,
TypeScript, VB), **DeepWiki MCP** to read a language's official spec/reference
repo on GitHub, and **Context7** for library and framework docs.

Use `WebSearch` to locate the official spec/reference, `WebFetch` to read it.
**Rule:** every factual cell must trace to a source. If a value cannot be
verified, omit that row — never guess. Resolve conflicts toward the official
spec, and note the version when a fact is version-specific.

### Phase 2 — Walk & fill

Go top-to-bottom through `TEMPLATE.md` in its given order. For each `SECTION`:

- Evaluate `include_if` / `omit_if` against the language. `omit_if` wins. If
  unsure whether a feature exists, **verify it (Phase 1)** — never assume.
- Keep → fill by `format`:
  - `table` → use the exact columns from its `table_schema`, in order. One row
    per item. Cells terse.
  - `prose` → a bulleted list of feature atoms: one `- **Name** — one or two
    sentences of purpose` list item per atom, plus an optional short snippet.
    Never stack bare atoms as consecutive lines — GFM collapses them into a
    run-on paragraph. Indent wrapped continuation lines two spaces.
  - `mixed` → lead table, then a bulleted-atom list for nuance.
- Drop → delete the entire block (heading, body, comments). No empty heading.
- **Paradigms:** copy the applicable canonical definitions from the template's
  menu verbatim, each with a `Here:` clause.
- **Code:** one statement per line, ≤ 64 columns, never wrapping. Shorten or
  split instead of wrapping.
- Use `> [!WARNING]` / `> [!NOTE]` for gotchas and idioms; `<details>` for long
  secondary lists (e.g. an exhaustive type table).

### Phase 3 — Assemble

- Headings: `#` language title, `##` Contents, `##` each Part, `###` each
  section. The header (title + badges) sits above the Parts.
- Drop any Part whose every section was omitted.
- Build **Contents** as nested bullets: one bold, unlinked top-level bullet per
  surviving Part, with an indented sub-bullet for each surviving section, in
  order, anchor-linking its `###` heading. Use the section's full title as the
  link text, verbatim.
- Strip **all** directive comments, `PART` markers, and `{{placeholders}}`.

### Phase 4 — Verify (gate; all must pass)

Check the output against `bootstrap.yaml.output_invariants`:

- [ ] `no_placeholders` — no `{{...}}` remain.
- [ ] `no_directives` — no `<!-- ... -->` remain.
- [ ] `order_preserved` — parts and sections follow template order.
- [ ] `parts_grouped` — sections nest under their Part (H2 > H3).
- [ ] `mandatory_present` — every `mandatory_sections` entry rendered.
- [ ] `toc_matches` — TOC equals surviving sections, in order.
- [ ] `toc_nested` — TOC is nested bullets (bold Part bullet + indented section sub-bullets).
- [ ] `anchors_valid` — every TOC link resolves to a heading.
- [ ] `prose_atoms_listed` — prose/mixed atoms are list items, not run-on lines.
- [ ] `code_wrap` — every code line ≤ 64 cols, one statement per line.
- [ ] `tables_schema` — each table uses its declared columns exactly.

Fix every violation and re-check. Do not present an unverified doc.

### Phase 5 — Write

Save to `languages/<lang>.md`. Report which sections were included, which were
omitted and why, and any rows dropped for lack of a verifiable source.

## Consistency guards

- Never reorder parts or sections. Never rename a section or a table column.
- Never invent table columns; use only the schema's columns.
- Same atom shape, same heading levels, same badge line in every doc.
- Mandatory sections are always present.

## Accuracy guards

- No fact in a table without a traceable source. Official spec wins conflicts.
- Omit unverifiable rows rather than guessing.
- State the target version; flag version-specific facts.

## Stop if you catch yourself

- Filling a primitives/operators/stdlib/tooling table from memory without
  checking docs → go back to Phase 1.
- Leaving a `{{placeholder}}` or a `<!-- comment -->` in the output.
- Wrapping a code line, or letting one run past ~64 columns.
- Keeping a heading with no content, or a TOC entry with no section.

See `reference/example-section.md` for the expected output shape.
