# Codocation IDE update feed

`updates.xml` is the Codocation IDE product-update feed consumed by the fork through
`CodocationExternalProductResourceUrls`. Its product code must match the fork's `IU` build code.

Do not add an IDE `<build>` entry until matching installers are published in a public
`codocation/codocation` release. Each entry must use the exact published build number, version,
release date, notes, and download destination. Until then, an empty `IU` product keeps update
checks valid without advertising an unavailable IDE release. Plugin and CLI assets in a `vX.Y.Z`
release do not authorize an `IU` installer entry.

Codocation plugins are distributed through JetBrains Marketplace. This directory does not host a
plugin repository and must not mirror or pin JetBrains plugins.

Publishing IDE installers and generating `updates.xml` remain manual until the installer release
contract is approved.
