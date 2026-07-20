---
title: Definitions Files
---

Definitions use one normalized topology. Root files under `definitions/` own stable IDs and
invariant fields. Locale files under `content/<locale>/definitions/` own translated payloads.
The default locale must provide a payload for every root ID; other locales may be sparse and
fall back by stable ID to the configured default locale.

## variables.yml

The root file owns each variable's stable `name`; the locale file owns its `name` and `value`:

```yaml
# definitions/variables.yml
variables:
  - name: product-name

# content/en/definitions/variables.yml
variables:
  - name: product-name
    value: Codocation
```

## glossary.yml

The root file owns the stable `id`; the locale file owns `id`, `term`, and `definition`:

```yaml
# definitions/glossary.yml
terms:
  - id: api

# content/en/definitions/glossary.yml
terms:
  - id: api
    term: API
    definition: "Application Programming Interface."
```

## keymaps.yml

The root file owns layout and action identity, platform data, shortcut values, and mappings.
The locale file supplies translated layout names and action descriptions:

```yaml
# definitions/keymaps.yml
id: default
layouts:
  - id: windows
    platform: PC
mappings:
  - key: ctrl
    value: Ctrl

# content/en/definitions/keymaps.yml
id: default
layouts:
  - id: windows
    name: Windows
```

## labels.yml

The root file owns label `id` and `color`; the locale file owns `id`, `name`, `compact`, and
`tooltip`:

```yaml
# definitions/labels.yml
labels:
  - id: stable
    color: "#10B981"

# content/en/definitions/labels.yml
labels:
  - id: stable
    name: Stable
    compact: Stable
    tooltip: Ready for publication
```

Locale data cannot introduce an unknown ID or change invariant fields. Structural definition
CRUD is available while editing the default locale; non-default locales edit only translated
payloads. Stable-ID renames update root data, all locale payloads, and supported page
references as one logical refactoring. Deleting a root ID never leaves dangling payloads or
references silently.

Labels remain IDE workflow metadata and are not published. Categories are derived from page
frontmatter; there is no categories definition file.
