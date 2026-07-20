---
title: Variables
---

A variable is a named value you define once and reuse across pages: a product name, a version,
or a support address. Change the value in one place, and every page that uses it updates on the
next build. The stable variable name is in root `definitions/variables.yml`; the selected
locale's value is in `content/<locale>/definitions/variables.yml`.

## Define

```yaml
# definitions/variables.yml
variables:
  - name: product-name
  - name: min-ide-version

# content/en/definitions/variables.yml
variables:
  - name: product-name
    value: Codocation
  - name: min-ide-version
    value: "2026.1"
```

The default locale must provide every root variable. A non-default locale may override only
the translated payload and falls back by stable name when it omits one.

## Use

Reference a variable anywhere in a page body as `{{name}}`:

```markdown
{{product-name}} requires IntelliJ IDEA {{min-ide-version}} or newer.
```

The value is substituted when the page is rendered: in the preview, on the built site, in the
PDF, and in search results and excerpts. Substitution works in body text, headings, and links,
but never inside fenced code blocks or inline code. A `{{name}}` that matches no defined
variable stays literal so a typo remains visible. Escape a defined variable as `\{{name}}` when
literal text is required.

The editor marks variable usages in the gutter, so you can see at a glance where a value is
reused. Renaming a variable updates its stable ID and all supported `{{name}}` references as a
single refactoring.
