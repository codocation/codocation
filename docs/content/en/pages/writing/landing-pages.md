---
title: Landing Pages
---

A landing page is a Markdown page with `layout: full` in its frontmatter. The layout drops
the sidebar chrome, and the body mixes ordinary Markdown prose with full-width sections
written as `:::name {…}` containers. The [home page](../index.md) of this site is a landing
page; open its source for a complete example.

## Create a landing page

Set `layout: full` and write sections straight into the body:

```markdown
---
title: My Product
layout: full
---

:::hero {badge="Free & open source" heading="Docs that live with your code."}
Turn a folder of **Markdown** into a documentation site.

[Get Started](getting-started.md){button="primary"}
:::

## Why My Product?

Ordinary Markdown between sections renders as a regular article column.

:::cta {heading="Ready to start?"}
[Get Started](getting-started.md){button="primary"}
:::
```

The frontmatter `title` still names the browser tab and search results; the visible page
heading comes from the hero section's `heading`.

## Section syntax

A section is a fenced container:

- `:::name {key="value"}` opens it; a bare `:::` closes the innermost container.
- Attributes are quoted `key="value"` pairs, nothing else.
- To nest, use the same fixed three-colon fence for parent and child: `:::cards` wraps
  `:::card` blocks, each closed by `:::`.
- Attributes use the canonical spaced form (`:::cta {heading="..."}`).
- A blank line after the opening fence is optional; the body starts on the next line.

The body between the fences is ordinary Markdown: paragraphs, **bold**, links, and
`{{variables}}` all work.

## Heading metadata

The same schema-driven attribute grammar applies to Markdown headings H1-H6. A terminal block may
contain scalar `label="id"`, token-list `categories="id-1, id_2"`, and the explicit anchor
shorthand `{#anchor}`. IDs use lowercase letters, digits, hyphens, and underscores. Spaces around
category items are ignored and formatting is emitted as comma-space; leading, trailing, or doubled
commas are errors. Anchors are unique within the page and unresolved classifier IDs offer a
create-definition quick fix.

```markdown
## Deploy {#deploy label="new" categories="beta-testing, cloud_services"}
```

Three attributes are shared across sections:

- `heading` - the section's heading. The hero's renders as the page's single visible
  top-level heading, every other section's one level below; you never manage heading
  levels yourself.
- `badge` - a small pill shown above the heading, available on any section.
- `background` - background variant. `background="tinted"` puts the section on an
  alternate tinted band; see [Alternate section backgrounds](#alternate-section-backgrounds).

## hero

The opening section: badge, heading, an intro paragraph, and the primary buttons.

```markdown
:::hero {badge="IntelliJ plugin · Standalone app" heading="One documentation studio. In your IDE or on its own."}
Codocation turns a folder of Markdown into a documentation site. Author with live preview,
arrange navigation visually, catch broken links as you type, and ship a static site or PDF.

[Get Started](getting-started/){button="primary"}
:::
```

| Attribute    | Required | Description                                  |
|--------------|----------|----------------------------------------------|
| `heading`    | yes      | The page's visible main heading.             |
| `badge`      | no       | Pill above the heading.                      |
| `background` | no       | `"tinted"` for an alternate background band. |

## cards

A grid of cards. The outer `:::cards` container holds one `:::card` per card; a
card's body is its description.

```markdown
:::cards {badge="Features" heading="Everything you need to write and ship docs"}
:::card {title="Live preview"}
See your Markdown rendered instantly in a side panel, with scroll sync, as you type.
:::
:::card {title="Visual navigation editor"}
Build the table of contents by drag-and-drop. Reorder, group, hide pages, and set the home page.
:::
:::
```

`cards`:

| Attribute    | Required | Description                                  |
|--------------|----------|----------------------------------------------|
| `heading`    | no       | The section heading.                         |
| `badge`      | no       | Pill above the heading.                      |
| `background` | no       | `"tinted"` for an alternate background band. |

`card`:

| Attribute | Required | Description                                                     |
|-----------|----------|-----------------------------------------------------------------|
| `title`   | yes      | The card title.                                                 |
| `image`   | no       | Image shown above the title.                                    |
| `href`    | no       | Destination linked from the card title.                         |

## steps

A numbered sequence of cards, same nesting as cards. Cards are numbered 1..N
automatically; a step with an `icon` shows the icon instead of a number and does not
consume one, so the numbered flow continues around it.

```markdown
:::steps {badge="How it works" heading="From an empty project to a published site in three steps"}
:::step {title="Create documentation"}
Open the Codocation tool window and choose Create Documentation.
:::
:::step {title="Write and preview"}
Add pages, arrange the navigation tree, and watch the live preview update as you type.
:::
:::step {title="Publish"}
Build the static site, export a PDF, or deploy when you are ready to ship.
:::
:::
```

`steps`:

| Attribute    | Required | Description                                  |
|--------------|----------|----------------------------------------------|
| `heading`    | no       | The section heading.                         |
| `badge`      | no       | Pill above the heading.                      |
| `background` | no       | `"tinted"` for an alternate background band. |

`step`:

| Attribute | Required | Description                                                                     |
|-----------|----------|---------------------------------------------------------------------------------|
| `title`   | yes      | The card title.                                                                 |
| `icon`    | no       | Replaces the card's number with an image; the card is skipped by the numbering. |

## Code examples

Shows off a file format or API with a normal fenced code block and syntax highlighting.

````markdown
Pages are Markdown files with YAML frontmatter.

```markdown
---
title: Installation
---

## Requirements
```
````

## cta

The closing call to action: a heading, a short line of text, and a button.

```markdown
:::cta {heading="Ready to document your project?"}
Create your first documentation in minutes.

[Get Started](getting-started/){button="primary"}
:::
```

| Attribute    | Required | Description                                  |
|--------------|----------|----------------------------------------------|
| `heading`    | yes      | The section heading.                         |
| `background` | no       | `"tinted"` for an alternate background band. |

## Buttons

Inside a `hero` or `cta` body, a link becomes a button when a `button` attribute is glued to its
closing parenthesis:

```markdown
[Get Started](getting-started/){button="primary"}
[View on GitHub](https://github.com/example/repo){button="secondary"}
```

`primary` is the filled accent button, `secondary` the outlined one. The `{...}` must
follow the `)` with no space; with a space in between it stays literal text. A link
without the attribute renders as a plain link.

## Alternate section backgrounds

By default every section sits on the page background. Add `background="tinted"` to a
section to place it on a subtly tinted band, so neighboring sections alternate instead of
blending into one solid block:

```markdown
:::cards {badge="Features" heading="Everything you need" background="tinted"}
:::card {title="Live preview"}
See your Markdown rendered instantly, with scroll sync, as you type.
:::
:::
```

The tint follows the site theme, so the same page alternates correctly in both light and
dark mode. `background` accepts only `"tinted"` and applies to whole sections; cards
(`:::card`, `:::step`) take their look from their parent section.

## Prose between sections

Any Markdown outside the containers - paragraphs, `##` headings, lists, images - renders
as a regular article column between the full-width sections. Use it for the explanatory
copy search engines and readers expect below the fold; any amount, anywhere between
sections, is fine.

## Variables

`{{name}}` references resolve in section bodies and in the prose between sections, exactly
as on any other page. Two rules to remember:

- Inside fenced code blocks and inline code, `{{name}}` is never substituted, so a code
  section can show template syntax literally.
- `\{{name}}` in regular text renders a literal `{{name}}`.
