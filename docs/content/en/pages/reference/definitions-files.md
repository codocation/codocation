---
title: Definitions Files
---

Definitions use one normalized topology. Root files under `definitions/` own stable IDs and
invariant fields. Locale files under `content/<locale>/definitions/` own translated payloads.
IDs are YAML mapping keys and are not repeated as `id:` values. The project-wide source for
translated-definition fallback is `definitions.fallbackLocale` in `codocation.yml`; it is
independent of every site's routing default.

## Labels and categories

```yaml
# definitions/labels.yml
labels:
  new:
    color: "#10B981"

# definitions/categories.yml
categories:
  beta-testing:
    color: "#2563EB"
  cloud_services: {}
```

```yaml
# content/en/definitions/labels.yml
labels:
  new:
    name: New
    compact: NEW
    tooltip: Recently added

# content/en/definitions/categories.yml
categories:
  beta-testing:
    name: Beta testing
    tooltip: Features available for early evaluation
  cloud_services:
    name: Cloud services
```

Root `color` is global. `name` and `tooltip` are localized; label-only `compact` is also
localized. A local non-fallback entry requires `name`; an optional field omitted from a local entry
remains absent in that locale.

## Locale availability and fallback

Each locale has exactly one of these states for a root identity:

1. No entry: unavailable in that locale.
2. A local entry: wholly local, with required `name` and optional `compact`/`tooltip`.
3. An entry containing only `translation: fallback`: use the complete entry from
   `definitions.fallbackLocale`.

These states are explicit. A missing entry stays unavailable; Codocation does not synthesize a
fallback without the marker. The marker enables fallback but does not choose the source; the
configured `definitions.fallbackLocale` is always the source.

```yaml
# content/de/definitions/labels.yml
labels:
  new:
    translation: fallback
```

Fallback cannot be combined with local fields, used in `definitions.fallbackLocale`, or used
without a root identity and valid fallback payload. Each case is an ERROR with a targeted quick
fix. Creating from Catalog writes the root identity and selected-locale payload atomically; adding
a locale creates fallback entries only for identities available in the configured definitions
fallback locale. Editing inherited text first materializes a complete local entry and removes
`translation: fallback`.

## Other definition files

Variables, glossary, and keymaps retain their domain-specific fields:

```yaml
# definitions/variables.yml
variables:
  - name: product-name

# content/en/definitions/variables.yml
variables:
  - name: product-name
    value: Codocation
```

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

Structural CRUD of global IDs and colors, including stable-ID rename, is available from any selected
locale. Localized text edits affect the selected locale; global operations update root data, locale
keys, and all supported page and heading
references. Locale-only deletion removes that locale's payload and references; global deletion
removes the root, every payload, and every reference. Removing the last page assignment does not
delete a definition.
