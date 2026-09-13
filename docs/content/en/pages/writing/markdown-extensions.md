---
title: Markdown Extensions
description: The attribute blocks, code-block options, image options, footnotes, and inline elements Codocation adds to ordinary Markdown.
---

A page body is ordinary Markdown. What follows is everything Codocation adds to it that is not a
`:::` container - those are in [Content Blocks](content-blocks.md) and
[Landing Pages](landing-pages.md).

Every addition below is written the same way: a brace block at the end of the line the thing sits
on, holding `key="value"` pairs. Nothing else changes; a file with no brace blocks is plain
Markdown and renders as plain Markdown everywhere.

## Attribute blocks

```markdown
# Billing {#billing .lead summary="What we charge for and when"}
```

A block may carry `key="value"` pairs, whitespace-separated `.class` tokens, and one `#anchor`.
Classes append to the classes the theme already writes on the element - they never replace them -
and land on the outermost box the carrier renders: the `<details>` wrapper of a collapsed fence or
list, an admonition's root block, a heading's own element, otherwise the carrier's outermost box.
The `semantic-` and `cdc-` prefixes belong to the theme and the product: a class using one is
reported (`SEM_038`) and not applied, while the other classes in the same block still apply. Style
your own classes through `web.customization.css`.

A block holding nothing but classes belongs to the carrier that owns the line it ends - a
definition term, a heading, any other block carrier - and a trailing inline link on that line stays
plain. Only where no block carrier hosts the line does an inline link claim the block. A keyed
block is never ambiguous: `button` claims its link wherever it appears.

Classes belong to the HTML alone. The plain projection strips them with the rest of the modifier
text, so search, excerpts, PDF, and the AI exports never see them.

## Code blocks

A fenced block takes its options after the language:

````markdown
```kotlin {title="Client.kt" line-numbers="true" emphasize-lines="3,7-9"}
val client = Client(token)
```
````

| Attribute         | Value                    | What it does                                                    |
|-------------------|--------------------------|-----------------------------------------------------------------|
| `title`           | text                     | Names the block above its body.                                  |
| `collapsed`       | `true` / `false`         | Renders the block inside a `<details>`, closed or open.           |
| `collapsed-title` | text                     | The summary line shown while it is closed.                       |
| `src`             | a `library/code/` path   | Takes the body from a file instead of the fence.                  |
| `lines`           | `3`, `3-9`, `1,4-6`      | Which lines of `src` to include.                                  |
| `line-numbers`    | `true` / `false`         | Numbers every line in the gutter.                                 |
| `emphasize-lines` | `3`, `3-9`, `1,4-6`      | Tints those lines of the rendered body.                           |
| `prompt`          | text                     | Draws that string as a prompt before each line, e.g. `$`.         |
| `variables`       | `true` / `false`         | `false` leaves `{{name}}` references in this block unresolved.    |
| `url-links`       | `true` / `false`         | Turns literal `https://` text in the block into links.            |
| `page-links`      | `true` / `false`         | Turns internal `.md` paths in the block into links.               |

Line specifications count the body's own lines from 1 and accept single numbers, `first-last`
ranges, and comma-separated lists of either. A range past the end of the block is reported
(`SEM_037`) and the rest still applies.

### Including a file

`src` resolves only inside `library/code/`, which is project-global and never published. The fence
body must be empty - the file is the body:

````markdown
```kotlin {src="library/code/client/Client.kt" lines="12-24" title="Client.kt"}
```
````

Authored ranges are concatenated in the order you write them, and the fragment is dedented so a
method lifted out of a class does not arrive indented by its class.

### Links inside code

A URL or a `.md` path inside a code block is inert unless the block asks otherwise. The two halves
switch separately on purpose: inside code, a path-shaped token is usually an argument, and only the
page half carries a validation obligation - a `page-links` block's broken path is reported as an
ordinary broken link (`MD_004`, `MD_008`), while `url-links` validates nothing.

### Inline code

An inline span takes one option, `variables`, so a sample of the syntax itself can show
`{{name}}` rather than its value:

```markdown
Write `{{product}}`{variables="false"} to substitute the product name.
```

Inside a carrier that declares `variables="false"`, nothing at all is rewritten - the `\{{name}}`
escape keeps its backslash too, because an escape corrupts a sample the reader is meant to copy.

## Images

An inline image takes its options the same way:

```markdown
![Editor panel](../../images/editor.png){width="720" border="line" caption="The editor panel"}
```

| Attribute | Value                 | What it does                                              |
|-----------|-----------------------|-----------------------------------------------------------|
| `width`   | a positive number     | Intrinsic width in pixels.                                 |
| `height`  | a positive number     | Intrinsic height in pixels.                                |
| `caption` | text                  | A caption under the image; requires standalone placement.  |
| `preview` | an image path         | The poster frame of a local video file; ignored elsewhere. |
| `border`  | `line`                | Draws a hairline frame around the image.                   |

An image written inside a line of prose is sized to that line instead of to its own pixels, so an
inline icon sits on the text baseline at text size without any attribute.

### Dark-theme images

Drop `panel_dark.png` beside `panel.png` and the dark theme uses it. There is nothing to write: the
convention reaches every image the site paints, including card icons and footer marks, which are
never authored as inline Markdown and so could never carry an attribute. With no sibling the light
image is used in both themes, and that is not a diagnostic.

Only a local path with an extension can have a dark sibling; a remote URL cannot, because the
sibling could only be guessed, and a path that already ends in `_dark` does not get one of its own.

### Video and embeds

Video is detected from the source and needs no attribute block: the extensions `.mp4`, `.webm`,
`.mov`, `.m4v`, `.ogv`, and YouTube and Vimeo URLs all embed a player. A remote video that does not
stand alone in its paragraph degrades to its link and warns; a local one stays a player inline.

An external page embeds through a directive, which requires a title and an http(s) URL and must
stand alone on its line:

```markdown
{% frame "https://example.com/demo" title="Live demo" %}
```

## Footnotes

Standard footnote syntax. A reference is `[^id]` and its definition is `[^id]: text` anywhere in the
page:

```markdown
Deployment is atomic[^atomic].

[^atomic]: Either every file of the new build is served, or none of it is.
```

Definitions are collected into one section at the end of the page under a heading that is
locale-owned chrome, not authored. A reference with no definition is an error (`MD_017`); a
definition nothing references (`MD_018`) and a second definition of one id (`MD_019`) are warnings,
and the first definition is the one used.

Footnote tokens are never live inside code, whatever a block says about `variables`.

## UI paths

`<ui>` names a control or a menu path the reader has to find in an interface:

```markdown
Open <ui>Settings > Tools > Codocation</ui> and switch the theme.
```

Segments are separated by `>` and each is drawn as its own chip with a separator glyph between
them. The separator is a real character in the markup, so copy-paste and PDF keep the path intact,
and a screen reader is given the whole path as one phrase. The body is literal text: no Markdown is
parsed inside it, because a control's label is a string.

An element that is not closed on the same line reports `SEM_036` and stays as text.

## Lists

A list takes presentation options on the line above it:

```markdown
{type="a" columns="2" sorted="asc"}
- Alpha
- Beta
```

| Attribute | Value                                        | What it does                          |
|-----------|----------------------------------------------|---------------------------------------|
| `type`    | `1`, `a`, `A`, `i`, `I`, `disc`, `circle`, `square` | The marker the list counts or bullets with. |
| `columns` | `2`, `3`, `4`                                | Lays the items out in that many columns. |
| `sorted`  | `asc`, `desc`                                | Sorts the items instead of keeping the authored order. |

A value outside a vocabulary is reported once and the list renders as it would have without the key.

## Links

An inline link takes two options:

```markdown
[Get started](/start/){button="primary"}
[The manual](../../attachments/manual.pdf){download="true"}
```

`button` draws the link as a call to action - `primary` filled, `secondary` outlined, the same two
names a header entry's `type: primary-button` uses in [tree.yml](../reference/tree-yml.md).
`download="true"` asks the browser to save the target instead of navigating to it.
