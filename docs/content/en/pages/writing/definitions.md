---
title: Glossary and Keymaps
---

Alongside [variables](variables.md), the "Definitions" tool window manages normalized
definition collections. Root `definitions/` files own stable identity and invariant fields;
the selected locale's `content/<locale>/definitions/` files own translated payloads.

## Glossary

The root record provides the stable `id`, while the locale provides the translated term and
definition:

```yaml
# definitions/glossary.yml
terms:
  - id: api

# content/en/definitions/glossary.yml
terms:
  - id: api
    term: API
    definition: "Application Programming Interface - a set of routines, protocols, and tools."
```

Catalog locales may provide only the records they translate. A missing payload is unavailable in
that locale; it falls back by stable ID only when the entry explicitly contains
`translation: fallback`. The marker enables fallback but does not choose the source: the complete
payload always comes from `definitions.fallbackLocale`, independent of the requesting site.
An unknown ID or a changed invariant field is an ERROR diagnostic.

## Keymaps

The root `definitions/keymaps.yml` describes layouts, platforms, shortcut values, and mappings.
The locale payload in `content/<locale>/definitions/keymaps.yml` supplies translated layout
names and action descriptions. Define a shortcut once and reference it by id when writing
about it, so platform differences stay in one file instead of being retyped in every page.

## Editing

Structural definition CRUD of global IDs and colors is available from any selected locale. Creating
an ID writes the root invariant and the selected-locale payload together. Localized text edits affect
only the selected locale; deleting a local payload makes that identity unavailable until an explicit
fallback entry is added. The underlying YAML remains plain and diff-friendly, so hand edits and code
review work as usual.
