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
- a site's `defaultLocale` selection is invalid;
- a required declared site/locale tree is absent;
- a non-default page marked `translation: fallback` has no site-default source.

Everything else is collected as a diagnostic on the snapshot. The IDE keeps the last valid state
when one exists, and the CLI build/validate gate fails on ERROR diagnostics. Examples include:

- a `translated` page missing in the requested locale;
- `translation: fallback` in the default tree or next to an existing requested-locale file;
- a missing image or attachment after source-aware resolution;
- an invalid relative link or unsupported `assets/` namespace;
- unknown locale definition IDs, missing site-default payloads, or changed invariant fields;
- unresolved label/category IDs, invalid IDs, localized fields in root definition files, and
  fallback entries mixed with local fields;
- malformed typed category lists (empty items), duplicate explicit anchors, and pages without an
  effective title;
- split title metadata between frontmatter and the first H1, differing singular values, repeated
  labels, and repeated category IDs (warnings when rendering remains deterministic);
- a locale-shaped directory that is not configured in `codocation.yml` (an orphan-locale ERROR;
  files and non-locale-shaped directories are ignored).
- a tree or sidecar for an undeclared site/locale pair (an orphan-pair ERROR; the file is ignored);
- a root catalog locale unused by every site (strict union ERROR; inactive entries are invalid).

`translation: fallback` enables fallback but does not choose the source. Pages and sidecars use the
site's `defaultLocale`; localized definitions use that same effective site default. Free ordinary
operation reads each site's default variant, while Pro reads all explicit variants and the CLI
continues to validate and build all variants. Locale management always reads the complete authored
project.

Classifier and heading diagnostics provide focused quick fixes: create a missing definition,
remove a repeated declaration, canonicalize comma-space lists, rename/remove duplicate anchors,
or move the complete title bundle atomically to frontmatter or the first H1. The category union is
computed once before either split-title fix, so both choices preserve the same stable order.

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
