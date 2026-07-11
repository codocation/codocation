---
title: Glossary and Keymaps
---

Alongside [variables](variables.md), the "Definitions" tool window manages two more shared
collections: the glossary and the keymaps. Both are authoring aids that keep terminology and
shortcut names consistent while you write.

## Glossary

`commons/glossary.yml` holds your project's term definitions:

```yaml
terms:
- term: API
  definition: "Application Programming Interface - a set of routines, protocols,
    and tools for building software applications."
```

Keep every term your documentation relies on defined here: the glossary is the single place
a reader of your sources (and a teammate writing a new page) checks what a term means in
this project.

## Keymaps

`commons/keymaps.yml` describes keyboard shortcuts per layout (Windows, macOS, Linux), with
mappings from key names to their display form (`cmd` renders as the ⌘ glyph, `ctrl` as
Ctrl, and so on). Define a shortcut once and reference it by id when writing about it, so
platform differences stay in one file instead of being retyped in every page.

## Editing

Both collections are edited in the "Definitions" tool window; the underlying YAML in
`commons/` is plain and diff-friendly, so hand edits and code review work as usual.
