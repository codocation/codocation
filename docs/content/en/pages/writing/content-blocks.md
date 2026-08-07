---
title: Content Blocks
---

Beyond plain Markdown, a few containers give in-page content its own structure: tabbed
panels, side-by-side code comparisons, and a set-apart summary aside. Unlike the
landing-page sections in [Landing Pages](landing-pages.md), which render as boxed or
full-width bands, these sit inline in the article column like the Markdown around them.

## tabs

A set of tabbed panels. The outer `:::tabs` container holds one `:::tab` per panel, and
needs at least one; a tab's body is its panel content.

```markdown
:::tabs
:::tab {title="Windows"}
Run `install.bat` from an elevated prompt.
:::
:::tab {title="macOS"}
Run `./install.sh` from a terminal.
:::
:::
```

`tabs` takes no attributes.

`tab`:

| Attribute | Required | Description                            |
|-----------|----------|----------------------------------------|
| `title`   | yes      | The tab's visible label.               |
| `key`     | no       | Syncs same-keyed tabs across the page. |

A tab's `key` links it to every same-keyed tab in every other `:::tabs` group on the page:
activating one activates them all, the choice persists across visits, and `?tabs=key1,key2`
in the URL preselects it. A key must match `[a-z0-9][a-z0-9-]*`, and repeating one within
the same `:::tabs` group is an error, same as a malformed one.

## compare

Two fenced code blocks side by side. The body must be exactly two fences - nothing else,
no other content, no other count.

````markdown
:::compare
```java {title="Java"}
System.out.println("Hi");
```

```kotlin {title="Kotlin"}
println("Hi")
```
:::
````

Each pane's title comes from the fence's own `title` attribute, falling back to `src`,
then the language, then nothing. `compare`:

| Attribute | Required | Description                                           |
|-----------|----------|-------------------------------------------------------|
| `layout`  | no       | `"columns"` (default) or `"rows"` to stack the panes. |

## tldr

A set-apart quick-summary aside. It renders identically no matter where it is written -
placement never changes the output. Where within a section it sits is only advised, not
enforced (see below); but a tldr must be a root container - never nested inside a `tab`,
`card`, `step`, `hero`, `cta`, or `compare` - and `SEM_008` reports that as a
build-blocking error, not a warning.

```markdown
## Deploying to production

:::tldr
Run `codocation build` then `codocation deploy prod`. Rollback with `--rollback`.
:::

The rest of this section walks through each flag in detail.
```

Where it sits within a section is about usefulness, not function: directly under its
heading, a tldr gives the reader something to act on before they read the section; lower
down, it is just a set-apart aside rather than a summary. Two warnings point at this -
`SEM_034` when a tldr is not the first content in its section, `SEM_035` when a second tldr
appears in the same section - and both are warnings, not build-blocking errors. A section
with two `tldr` containers renders both; the first is the one that answers "what is this
section about".

`tldr` takes no attributes.
