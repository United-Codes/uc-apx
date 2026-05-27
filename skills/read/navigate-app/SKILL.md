---
name: navigate-app
description: Orient yourself in an unfamiliar Oracle APEX apexlang project using the `uc-apx` CLI. Use at the start of any task on a new apexlang codebase, or when you need to find where a feature lives before reading files. Maps user questions ("which page is X on?", "what LOVs exist?") to the right `uc-apx` subcommand instead of grepping blind.
---

# Navigating an apexlang application

When you land in an apexlang project (`.apx` files, usually with `application.apx` at the root), don't grep. The `uc-apx` CLI maintains an in-memory index across all files and answers structural questions cheaply.

## When to use this skill

- First contact with a new apexlang project — establish shape before reading.
- The user asks a structural question: "what pages exist?", "what's on page 4?", "what shared components are defined?".
- You need to locate the file containing a known construct so you can edit it.

**Do not** use this skill when:

- The user asks "why is X behaving wrong?" — that's [skills/read/investigate-component/SKILL.md](../investigate-component/SKILL.md).
- The user already pointed you at a specific file or page — just read it.

## Decision tree

```
First question?
├── "What's this app?"             → uc-apx overview
├── "What pages exist?"            → uc-apx pages
├── "What's on page N?"            → uc-apx tree N        (structure)
│                                    uc-apx page N        (full detail)
├── "What LOVs / lists / ... ?"    → uc-apx list <kind>
└── "Where is component X?"        → uc-apx component <id-or-name>
```

## Command reference

All commands honor `--app-dir <root>` (default `.`). Default output is **minified JSON** — no flag needed for agents. Use `--json-pretty` for indented JSON or `--toon` for TOON format.

### Overview — app shape at a glance

```bash
uc-apx overview --app-dir <root>
```

Returns: app name, file count, component counts by kind (e.g. `page: 49, region: 142, pageItem: 53`). Use as your first call to size the project.

### Pages — the page index

```bash
uc-apx pages --app-dir <root>
```

Returns one row per page with `id`, `name`, `alias`, `title`, `file`, and total component count. Skim this to find candidate pages by name/alias.

### Tree — one page's structure

```bash
uc-apx tree <id|alias|name> --app-dir <root>
```

Returns a compact hierarchical view: regions → their items/buttons → child regions. Best when you want to know *what's on* a page without dumping every property.

### Page — full page detail

```bash
uc-apx page <id|alias|name> --app-dir <root>
```

Returns everything: regions, page items, buttons, processes, computations, validations, branches, dynamic actions, plus flattened properties. Use after `tree` once you've picked the page you care about.

### List — enumerate shared components

```bash
uc-apx list                 # all top-level constructs
uc-apx list <kind>          # filter by kind: lov, list, breadcrumb, authentication, ...
```

Use to inventory what shared resources the app already provides before you propose adding new ones.

### Component — fetch any node

```bash
uc-apx component <id|name> --app-dir <root>
```

Returns the full component tree (children + flattened properties) for any APEX$<digits> id or human name. The general "I know exactly what I want" lookup.

## Worked example — orienting in `examples/brookstrut`

```bash
uc-apx overview --app-dir examples/brookstrut
# → 49 pages, 142 regions, 17 LOVs, etc.

uc-apx pages --app-dir examples/brookstrut | head
# → p00001 HOME, p00002 SALES-HISTORY-CONTENT-ROW, p00003, ...

uc-apx tree 2 --app-dir examples/brookstrut
# → region "Sales History" (type: classicReport), region "Filters", buttons …

uc-apx page 2 --app-dir examples/brookstrut
# → full property dump for page 2
```

By the time you've run those four commands, you know: how big the app is, how many pages, what's on the page you care about, and the exact source file. From here you can read the file directly, or stay in CLI mode.

## Common pitfalls

- **Don't grep across `.apx` files before trying `uc-apx search` / `uc-apx component`.** The index already covers names, IDs, and code-block contents.
- **Page identifiers are flexible.** `uc-apx page 4`, `uc-apx page SETTINGS`, and `uc-apx page "Administration"` all work. Use whichever the user gave you.
- **Default output is minified JSON.** No flag needed for agents — `json.loads()` works out of the box. Use `--json-pretty` for human inspection or `--toon` for TOON format.
- **`overview` doesn't enumerate every component.** It's a count. Use `list`, `pages`, or `search` to enumerate.

## Reference

- Commands: [cmd/](../../../cmd/)
- Index source: [index/index.go](../../../index/index.go)
- Output shapes: [output/json.go](../../../output/json.go)
