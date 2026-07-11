---
title: Commons Files
---

Shared definitions live under `commons/`, one YAML file per collection. The "Definitions"
and "Catalog" tool windows edit them; the formats below are what lands in the files.

## variables.yml

Values for `{{name}}` substitution in page bodies (see
[Variables](../writing/variables.md)).

```yaml
variables:
- name: product-name
  value: Codocation
```

## glossary.yml

Term definitions.

```yaml
terms:
- term: API
  definition: "Application Programming Interface."
```

## keymaps.yml

Keyboard shortcut definitions: layouts and key display mappings.

```yaml
id: default
layouts:
  - id: windows
    name: Windows
    platform: PC
  - id: macos
    name: macOS
    platform: MAC
mappings:
  - key: cmd
    value: "⌘"
  - key: ctrl
    value: Ctrl
```

## labels.yml

Workflow labels for the "Catalog" tool window: an id, a display name, and a color. Labels
stay in the IDE and are never published.

```yaml
labels:
- id: stable
  name: Stable
  color: "#10B981"
```
