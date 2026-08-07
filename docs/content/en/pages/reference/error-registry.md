---
title: error-registry.yml
---

Per-rule severity overrides for [validation](../testing/validation.md), in a single file at
the project root next to `codocation.yml`. The file holds only the rules you changed;
everything else uses the defaults below. This catalog mirrors the plugin's registered
`DiagnosticCode` values; only these codes are valid severity-override keys.

```yaml
severities:
  MD_004: error
  NAV_002: suppressed
```

Values: `error` (blocks build and deploy), `warning`, `weak-warning`, `suppressed` (the
rule stops reporting). The plugin's `Severity.INFO` is presented here as `weak-warning`.

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
| NAV_011 | warning | The documentation has no description |
| NAV_012 | error | No published page can serve the site root `/` |

### Markdown

| Code | Default | Checks |
| --- | --- | --- |
| MD_001 | error | The page has no title |
| MD_002 | error | The frontmatter is invalid |
| MD_003 | warning | An unknown frontmatter field (with a did-you-mean hint) |
| MD_004 | error | An internal path or namespace is invalid, or a canonical page, image, attachment, or asset target is missing |
| MD_005 | warning | A link points to a heading that does not exist |
| MD_006 | weak-warning | The title comes from frontmatter and the page still carries an H1 |
| MD_007 | warning | More than one top-level heading |
| MD_008 | error | A link target is not included in the current site's tree |
| MD_009 | error | A link target exists, but its location is not published |

### Configuration

| Code | Default | Checks |
| --- | --- | --- |
| CFG_001 | error | A configured branding or media file is missing, including the logo or favicon |
| CFG_002 | error | Two sites share one base path |
| CFG_003 | error | A configuration file has an invalid setting |

### Localization

| Code | Default | Checks |
| --- | --- | --- |
| I18N_001 | error | A page is missing from a requested locale |
| I18N_002 | error | A page is marked `fallback` in a site's effective default locale |
| I18N_003 | error | A page is marked `fallback` while a requested-locale file exists |
| I18N_004 | error | A locale-shaped directory is not configured |
| I18N_005 | error | A site tree or sidecar is present for an undeclared site/locale pair |
| I18N_006 | error | A catalog locale is not published by any site |

### Semantic Markdown

| Code | Default | Checks |
| --- | --- | --- |
| SEM_001 | error | A semantic-looking fence does not use exactly `:::` |
| SEM_002 | error | An opened container has no closing `:::` |
| SEM_003 | error | Unknown container name |
| SEM_004 | error | Malformed attribute token; expected `key="value"` or one `#anchor` |
| SEM_005 | warning | Attribute is not supported by that carrier |
| SEM_006 | error | Required attribute is missing |
| SEM_007 | error | A bare closing fence has no open container |
| SEM_008 | error | A child is outside its required parent or a grouping parent has invalid direct content |
| SEM_009 | warning | An enum attribute value is invalid |
| SEM_010 | error | A modifier is detached or attached to an unsupported carrier |
| SEM_011 | warning | Unknown admonition type renders neutrally |
| SEM_012 | error | Snippet ID is defined more than once |
| SEM_013 | error | Included snippet ID is missing |
| SEM_014 | error | Include graph contains a cycle |
| SEM_015 | error | Malformed, mismatched, or unclosed template directive |
| SEM_016 | error | A filter condition has no snippet/include scope |
| SEM_017 | error | Snippet ID, filter tag, or site token violates its grammar |
| SEM_018 | error | One attribute key or anchor is repeated on a carrier |
| SEM_034 | error | A tldr must be the first content in its section |
| SEM_035 | error | A section may hold only one tldr |

### Definitions

| Code | Default | Checks |
| --- | --- | --- |
| GLOS_001 | error | A glossary term is not defined |
| GLOS_002 | warning | A glossary term is defined more than once |
| KEYS_001 | error | A keyboard shortcut action is not defined |
| KEYS_002 | warning | A keyboard shortcut action is defined more than once |

## Failures without a stable registry code

Some structural validation and reader failures currently have no registered diagnostic code.
Unknown or incomplete definition payloads, invariant definition changes, and reader failures
such as an absent or unparseable `codocation.yml`, invalid site-default selection, a missing
required declared site/locale tree, or a fallback page with no site-default source are reported
outside the code table until a stable code is implemented. Use only the registered codes above
when writing `error-registry.yml` overrides.
