---
title: Validation
---

Codocation validates the project continuously: broken links, malformed configuration, missing
resources, and navigation problems surface while you write, not after readers find them.

## Where problems show up

- **In the editor**: findings are highlighted inline in Markdown and YAML files as you type,
  with quick fixes where one applies.
- **In the "Problems" tab**: the complete project snapshot, including provenance and locale
  diagnostics, so nothing hides in a file you do not have open.

## Reader failures and diagnostics

Reader failures mean the project cannot be constructed:

- `codocation.yml` is absent or unparseable;
- default-locale selection is invalid;
- a required site/locale tree is absent;
- a non-default page marked `translation: fallback` has no default-locale source.

The reader does not silently mix legacy paths with the canonical layout. Legacy paths are never
read, so a legacy project fails naturally when its required tree is missing.

Everything else is collected as a diagnostic on the snapshot. The IDE keeps the last valid state
when one exists, and the CLI build/validate gate fails on ERROR diagnostics. Examples include:

- a `translated` page missing in the requested locale;
- `translation: fallback` in the default tree or next to an existing requested-locale file;
- a missing image or attachment after source-aware resolution;
- an invalid relative link or unsupported `assets/` namespace;
- unknown locale definition IDs, missing default-locale payloads, or changed invariant fields;
- a locale-shaped directory that is not configured in `codocation.yml` (an orphan-locale ERROR;
  files and non-locale-shaped directories are ignored).

The checks also cover broken page links, missing images and attachments, malformed
configuration, navigation references, duplicate configuration, and frontmatter schema errors.

The CLI fails in both cases, but the distinction matters: an ERROR describes a broken snapshot,
while a reader failure means no current snapshot can be constructed.

## Severities and the build gate

Every finding has a severity. Findings with `error` severity block build and deploy. Override
rules in the project-global `error-registry.yml` next to `codocation.yml`; warnings and infos
do not block.

## The home-page guard

A docs site must have a home page: either a page marked as home in the navigation or a
top-level `pages/index.md`. Without it the build stops and names the missing requirement.
