---
title: Validation
---

Codocation validates the project continuously: broken links, malformed configuration, and
navigation problems surface while you write, not after readers find them.

## Where problems show up

- **In the editor**: findings are highlighted inline in the Markdown and YAML files as you
  type, with quick fixes where one applies.
- **In the "Problems" tab** of the Codocation tool window: the full list for the whole
  project, so nothing hides in a file you don't have open.

## What is checked

The checks cover the things that break a published site: page links whose target does not
exist, images that point nowhere, navigation tree entries referencing missing files,
duplicate or malformed configuration, frontmatter that does not match its schema.

## Severities and the build gate

Every finding has a severity. Findings with the "error" severity do more than warn: the
build and the deploy refuse to run while any exist, so a site with broken navigation cannot
be published by accident. Warnings and infos never block.

When a rule's default severity does not fit your project, override it in
`error-registry.yml` next to `codocation.yml`: per-rule severity overrides live there, in
one reviewable file.

## The home-page guard

One structural requirement is enforced at build time: a docs site must have a home page (a
page marked as home in the navigation, or a top-level `index.md`). Without it the site root
would be empty, so the build stops and names exactly what is missing.
