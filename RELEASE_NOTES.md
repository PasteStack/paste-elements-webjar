# paste-elements-webjar v0.3.0

**Date:** 2026-09-22

## Fixed

- The pipeline can run: the publish job no longer calls toolbox scripts that exist
  only inside the house runner images while running on a public Maven image.
- Branch pipelines publish the pinnable `{ref-slug}-{sha}` and rolling
  `{ref-slug}-latest` versions instead of redeploying the released coordinates.
- Version agreement (`VERSION`, the pom's `<version>`, and `<upstreamVersion>`) is
  read through Maven, so a future `<parent>` block cannot make the check validate
  the wrong element. A tag that does not match `VERSION` refuses to publish.
- The README described version 0.1.0 and a `src/js` / `src/scss` layout; the jar
  carries `base/`, `modules/`, and `structure/` under the upstream version.

## Licensing

- This repository's own build files are licensed under the Apache License,
  Version 2.0 (`LICENSE`, `NOTICE.md`).
- The pom's `<licenses>` now describes what the jar actually contains — the packaged
  `upstreamVersion` — instead of this repository. It moves to Apache-2.0 in the same
  change that raises `upstreamVersion` to the first relicensed `paste-elements`
  release.

# paste-elements-webjar v0.1.0

**Date:** 2026-02-07

## Initial Release

- Added `pom.xml` for webjar packaging of paste-elements
