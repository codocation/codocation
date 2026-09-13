---
title: RSS Feeds
---

A site can publish one RSS 2.0 feed for each locale variant. The feed carries that variant's
published, discoverable pages and orders them newest first by the date field the site type
displays. It is a feed for the site, not a selected section.

## Configure a feed

Set `web.feed` globally or under a site's `web` block in `codocation.yml`:

```yaml
web:
  feed:
    enabled: true
    limit: 20
    fullText: false
```

`enabled` turns the feed on for a site. `limit` caps the number of items and defaults to `20`;
`fullText` defaults to `false`, so items carry the page annotation rather than the full body.
There is no feed-specific title setting: each feed uses the title of its own locale variant.

Set `publicUrl` to an absolute URL with both a scheme and a host so the feed can be written and its
pages can announce it; without one, no feed is written for that site, none of its pages announce
one, and `CFG_028` reports the missing URL as a warning. A bare `http://` has a scheme but no host
and does not satisfy this requirement.

## Publications and updates

On a blog, the feed is a publication stream. An item carries the date its post was published,
and editing the post does not move it.

On a docs site, the feed is an update stream. An edited page reappears because being edited is
the event the feed reports. A docs feed is not a changelog or a stream of new pages.

These dates have two direct consequences. A docs page reappears whenever anything moves its
`updated` date, including the commit that first publishes it and the commit that renames its
file when either commit lands on a later day than its last `updated` day. A blog post that sat
unpublished keeps the `published` date its history resolves to, so it may land below the limit
on the day it goes live.

By default, the channel description repeats this distinction in the variant's resolved site
language: a blog channel says `{title} is a publication stream.` and a docs channel says
`{title} is an update stream.` The locale's `content/<locale>/site-strings.yml` supplies the
localized wording.

## Item identity

An item is identified by the page's own URL in its locale variant. If that URL changes because
the page is renamed, re-slugged, or moved, it is a different item afterwards; a reader holding
the old item may show both.

A change that rewrites every URL at once does this to the whole feed in one step. Examples are
a new base path, a new `publicUrl`, or toggling `cleanUrls`.

RSS guarantees nothing about what a reader does with an item it already holds. Whether changed
content or a changed date marks that item unread again is the reader's decision, so no Codocation
surface promises that an update is quiet.
