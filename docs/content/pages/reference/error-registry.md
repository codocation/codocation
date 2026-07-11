---
title: error-registry.yml
---

Per-rule severity overrides for [validation](../testing/validation.md), in a single file at
the project root next to `codocation.yml`. The file holds only the rules you changed;
everything else uses the defaults below.

```yaml
severities:
  MD_004: error
  NAV_002: suppressed
```

Values: `error` (blocks build and deploy), `warning`, `weak-warning`, `suppressed` (the
rule stops reporting).

## Rules and default severities

### Navigation

| Code | Default | Checks |
| --- | --- | --- |
| NAV_001 | error | A navigation entry points to a missing page |
| NAV_002 | warning | A page is in no navigation, so it will not appear on the site |
| NAV_003 | warning | The same page appears several times in one section |
| NAV_004 | error | The navigation file is malformed |
| NAV_005 | error | A nested section where only flat items are allowed (header/footer) |
| NAV_006 | error | An item combines page, section, and href |
| NAV_007 | error | A bare path used as a key instead of `page:` |
| NAV_008 | error | More than one page marked as home |
| NAV_009 | error | An item kind not allowed in this section |
| NAV_010 | error | An invalid navigation item field |

### Markdown

| Code | Default | Checks |
| --- | --- | --- |
| MD_001 | error | The page has no title |
| MD_002 | error | The frontmatter is invalid |
| MD_003 | warning | An unknown frontmatter field (with a did-you-mean hint) |
| MD_004 | error | A link points to a page that does not exist |
| MD_005 | warning | A link points to a heading that does not exist |
| MD_006 | weak-warning | The title is set twice (frontmatter and an H1) |
| MD_007 | warning | More than one top-level heading |

### Configuration

| Code | Default | Checks |
| --- | --- | --- |
| CFG_001 | error | The branding logo file is missing |
| CFG_002 | error | Two sites share one base path |

### Landing sections

| Code | Default | Checks |
| --- | --- | --- |
| LAND_001 | error | The legacy `sections:` frontmatter key (moved into body directives) |
| LAND_002 | error | A `:::` directive has no closing fence |
| LAND_003 | error | An unknown section name |
| LAND_004 | error | A malformed attribute (not `key="value"`) |
| LAND_005 | warning | An attribute the section does not recognize |
| LAND_006 | error | A required attribute is missing |
| LAND_007 | warning | An invalid button value (use "primary" or "secondary") |
| LAND_008 | error | A closing fence with no open directive |
| LAND_009 | warning | A card outside a features/steps container |
