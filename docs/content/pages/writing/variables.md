---
title: Variables
---

A variable is a named value you define once and reuse across pages: a product name, a
version, a support address. Change the value in one place, and every page that uses it
updates on the next build.

## Define

Variables live in `commons/variables.yml` and are edited in the "Definitions" tool window:

```yaml
variables:
- name: product-name
  value: Codocation
- name: min-ide-version
  value: "2026.1"
```

## Use

Reference a variable anywhere in a page body as `{{name}}`:

```markdown
{{product-name}} requires IntelliJ IDEA {{min-ide-version}} or newer.
```

The value is substituted when the page is rendered: in the preview, on the built site, in
the PDF, and in search results and excerpts. Substitution works in any body text, headings,
and links - but never inside fenced code blocks or inline code, so you can document
`{{placeholder}}` syntax in code samples without it being replaced.

A `{{name}}` that matches no defined variable stays in the text literally, so a typo is
visible in the preview instead of silently disappearing. To show a literal `{{name}}` in
regular text even when the variable is defined, escape it as `\{{name}}`.

The editor marks variable usages in the gutter, so you can see at a glance where a value is
reused.
