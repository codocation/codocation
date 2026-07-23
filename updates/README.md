# Codocation update feeds

`updates.xml` is the Codocation IDE update feed consumed by the fork through
`CodocationExternalProductResourceUrls`. Its product code must match the fork's `IU` build code.

Do not add an IDE `<build>` entry until matching installers are published in a public
`codocation/codocation` release. Each entry must use the exact published build number, version,
release date, notes, and download destination. Until then, an empty `IU` product keeps update
checks valid without advertising an unavailable IDE release.

`plugins.xml` is the custom plugin repository referenced by `CodocationApplicationInfo.xml`.
Marketplace update IDs and compatibility ranges must be verified against the exact IntelliJ
Platform build line used by Codocation.

Publishing IDE installers and generating `updates.xml` are intentionally manual until the release
artifact and versioning contract is approved.
