---
title: Content Blocks
---

Beyond plain Markdown, a few containers give in-page content its own structure: tabbed
panels, side-by-side code comparisons, and a highlighted summary aside. Unlike the
landing-page sections in [Landing Pages](landing-pages.md), these work in the body of any
page, not only `layout: full` ones.

## tabs

A set of tabbed panels. The outer `:::tabs` container holds one `:::tab` per panel; a
tab's body is its panel content.

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

| Attribute | Required | Description                                                          |
|-----------|----------|-----------------------------------------------------------------------|
| `title`   | yes      | The tab's visible label.                                             |
| `key`     | no       | Stable identifier rendered as a `data-tab-key` attribute on the tab.  |

## compare

Two fenced code blocks side by side. The body must be exactly two fences with a blank
line between them - nothing else, no other content, no other count.

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

| Attribute | Required | Description                                          |
|-----------|----------|-------------------------------------------------------|
| `layout`  | no       | `"columns"` (default) or `"rows"` to stack the panes. |

## tldr

A highlighted quick-summary aside. It renders identically no matter where it is written
or what is wrong with its placement - placement is never enforced, only advised.

```markdown
## Deploying to production

:::tldr
Run `codocation build` then `codocation deploy prod`. Rollback with `--rollback`.
:::

The rest of this section walks through each flag in detail.
```

Placement is about usefulness, not function: directly under its heading, a tldr gives the
reader something to act on before they read the section; lower down, it is just a
highlighted aside rather than a summary. Two warnings point at this - `SEM_034` when a
tldr is not the first content in its section, `SEM_035` when a second tldr appears in the
same section - and both are warnings, not build-blocking errors. A section with two `tldr`
containers renders both; the first is the one that answers "what is this section about".

`tldr` takes no attributes.
