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

Codocation runs 116 inspections over a project. `codocation rules` prints this same list from the
build you have installed, which is the copy to trust if the two ever disagree.

### Navigation and trees

| Code | Default | Checks |
| --- | --- | --- |
| NAV_001 | error | Navigation points to a missing page |
| NAV_002 | warning | A page is in no navigation tree, so it is never built |
| NAV_003 | warning | A page appears more than once in one navigation section |
| NAV_004 | error | The navigation file is malformed |
| NAV_005 | error | These navigation items must be flat - a nested section isn't allowed here |
| NAV_006 | error | A navigation item can't combine page, section, and href |
| NAV_007 | error | A navigation item uses a path as its key instead of 'page:' |
| NAV_008 | error | Only one page can be the home page |
| NAV_009 | error | This item isn't allowed in that navigation section |
| NAV_010 | error | Invalid navigation item field |
| NAV_011 | warning | This documentation has no description |
| NAV_012 | error | No published page can serve the site root '/' |
| NAV_013 | error | The route is reserved for archives |
| NAV_014 | warning | A brand is linked more than once in one navigation section |
| NAV_015 | warning | The item has no counterpart in the default language |
| NAV_016 | error | Two sections share an id |
| NAV_017 | error | 'section' takes no value; 'id', 'title' and 'children' are its siblings |
| NAV_018 | warning | 'inherited' can't be used in the default language, where every item is already this language's own |
| NAV_019 | error | The default-language navigation file is missing a required key |
| NAV_020 | warning | This locale writes matched entries in an order that contradicts the default language, so the default language's order is used |
| NAV_022 | warning | The announcement has no 'until' date, so it will stay up until it is removed by hand |
| NAV_023 | warning | The announcement's 'until' date isn't a valid date - in the default language it is treated as unset, but in a locale overlay the default language's own date is used instead |
| NAV_024 | warning | The local 'collapsed' value matches the site's navigation default |
| NAV_025 | warning | A membership-detach key can't be used in the default language, where there is no list to detach from |
| NAV_026 | warning | A membership-detach key with no list beside it detaches nothing, so the list stays inherited |
| NAV_027 | error | A blog table of contents entry must point under 'pages/posts/' |
| NAV_028 | warning | A link's href is a path, not a URL |
| NAV_029 | warning | A section id repeats across this language and the default language |

### Markdown pages

| Code | Default | Checks |
| --- | --- | --- |
| MD_001 | error | This page has no title |
| MD_002 | error | This page's frontmatter is invalid |
| MD_003 | warning | Unknown frontmatter field |
| MD_004 | error | This link points to a page that doesn't exist |
| MD_005 | warning | This link points to a heading that doesn't exist |
| MD_006 | warning | This page's title comes from frontmatter, so the H1 heading is a second title |
| MD_007 | warning | This page has more than one top-level heading (H1) |
| MD_008 | error | The link target isn't in this site's navigation, so the link would be broken |
| MD_009 | error | Invalid heading metadata |
| MD_010 | error | A referenced definition isn't defined for this locale |
| MD_011 | error | An anchor is used by more than one target |
| MD_012 | warning | Frontmatter and the first H1 disagree |
| MD_013 | warning | The label attribute is repeated on one target |
| MD_014 | warning | A category is repeated in one target |
| MD_015 | warning | One tag is written more than one way in one locale |
| MD_016 | error | A tag cannot be turned into a URL segment |
| MD_017 | error | A footnote reference has no definition |
| MD_018 | warning | A footnote is defined but never referenced |
| MD_019 | warning | A footnote is defined more than once |

### Project configuration

| Code | Default | Checks |
| --- | --- | --- |
| CFG_001 | error | A configured image is missing, so the site would ship a broken image |
| CFG_002 | error | One base path is used by more than one site |
| CFG_003 | error | This file has an invalid setting |
| CFG_005 | warning | A site lists the same locale twice |
| CFG_010 | warning | A locale code conflicts with another using the same case-insensitive identity |
| CFG_011 | warning | One locale title is used by several locale codes |
| CFG_012 | warning | Unknown site-strings key |
| CFG_013 | warning | A locale has no site-strings.yml, so its built-in text isn't translated |
| CFG_014 | warning | A locale's site-strings.yml is missing keys, so those labels aren't translated |
| CFG_015 | warning | A contributeUrl is not an absolute http(s) URL |
| CFG_016 | warning | A locale's site-strings.yml is complete but unmodified from English, so its built-in text isn't translated |
| CFG_024 | warning | A locale has no built-in text translation, so its site chrome renders English |
| CFG_025 | warning | Unknown diagnostic code in error registry |
| CFG_026 | warning | Invalid severity value in error registry |
| CFG_027 | warning | Error registry file is not valid YAML |
| CFG_028 | warning | Enabled feed has no absolute public URL |
| CFG_029 | error | A page output needs a feed path as a directory |
| CFG_030 | error | One category archive segment is used by more than one category |
| CFG_031 | warning | A variable payload names a site the project does not declare |
| CFG_032 | warning | The translation authors only a base value while the payload it shadows carries per-site values |
| CFG_033 | error | A base path contains an unsafe segment |
| CFG_034 | error | A public URL is not an absolute http(s) URL with a host |

### Semantic syntax

| Code | Default | Checks |
| --- | --- | --- |
| SEM_001 | error | A semantic fence must use exactly ':::' |
| SEM_002 | error | A semantic container has no closing ':::' |
| SEM_003 | error | Unknown semantic container |
| SEM_004 | error | Invalid semantic attribute - use key="value", #anchor, or .class |
| SEM_005 | warning | This attribute isn't supported on this semantic carrier |
| SEM_006 | error | A semantic container is missing a required attribute |
| SEM_007 | error | This closing fence doesn't match any open container |
| SEM_008 | error | A semantic container has invalid content |
| SEM_009 | warning | An attribute has an invalid value |
| SEM_010 | error | This semantic modifier is detached or unsupported |
| SEM_011 | warning | An unknown admonition type renders with neutral styling |
| SEM_012 | error | A snippet ID is defined more than once |
| SEM_013 | error | An included snippet does not exist |
| SEM_014 | error | Snippet includes form a cycle |
| SEM_015 | error | Malformed, mismatched, or unclosed template directive |
| SEM_016 | error | A filter condition requires a snippet or include scope |
| SEM_017 | error | This identifier, filter tag, or site token is invalid |
| SEM_018 | error | This attribute key or anchor is repeated |
| SEM_019 | error | A referenced code file does not exist under library/code |
| SEM_020 | error | Invalid line-range value - use N or N-M ranges separated by commas |
| SEM_021 | error | The requested lines are beyond the end of the file |
| SEM_022 | error | A code block with 'src' must have an empty body |
| SEM_023 | warning | 'preview' applies only to local video files |
| SEM_024 | warning | 'caption' requires the media to stand alone in its paragraph |
| SEM_025 | error | A frame requires a 'title' attribute |
| SEM_026 | error | A frame URL must use http or https |
| SEM_027 | error | A frame must stand alone on its line |
| SEM_028 | warning | A remote video must stand alone in its paragraph |
| SEM_029 | error | A tab key must be a lowercase slug of letters, digits, and hyphens |
| SEM_030 | error | Duplicate tab key in one tabs container |
| SEM_031 | error | A posts listing must stand alone on its line |
| SEM_032 | error | A page may hold only one posts listing |
| SEM_033 | error | A posts listing belongs to a blog site, not a docs site |
| SEM_034 | warning | A tldr must be the first content in its section |
| SEM_035 | warning | A section may hold only one tldr |
| SEM_036 | warning | A '<ui>' element needs text and a closing '</ui>' on the same line |
| SEM_037 | warning | Emphasized lines are past the end of the code block |
| SEM_038 | warning | A class uses a reserved prefix and is not applied |
| SEM_039 | warning | A switcher mark is nested inside another switcher section |
| SEM_040 | warning | A 'switcher' value cannot be blank |

### Glossary and shortcuts

| Code | Default | Checks |
| --- | --- | --- |
| GLOS_001 | error | A referenced glossary term isn't defined |
| GLOS_002 | warning | A glossary entry id is used more than once |
| KEYS_001 | error | A referenced keyboard shortcut action isn't defined |
| KEYS_002 | warning | A shortcut action is defined more than once |

### Locales and translation

| Code | Default | Checks |
| --- | --- | --- |
| LOC_004 | error | A locale directory is not configured |
| LOC_005 | error | A required navigation tree is missing for a site locale |
| LOC_006 | error | A tree or sidecar exists for an undeclared site locale |

## Failures without a stable registry code

Some structural validation and reader failures have no registered diagnostic code. Unknown or
incomplete definition payloads, invariant definition changes, and reader failures such as an absent
or unparseable `codocation.yml`, an invalid site-default selection, or a missing default-language
tree are reported outside the code table until a stable code is implemented. Use only the registered
codes above when writing `error-registry.yml` overrides.
