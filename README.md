# App development skills

Portable, English-language workflows extracted from `blurrf` and `kappan`. Each folder contains a standalone `SKILL.md` with a trigger description and executable checks. Follow the consuming repository's build files, version policy, signing configuration, and store requirements rather than copying a source project's paths or IDs blindly. The source projects are read-only inputs; this repository is the maintained copy.

## Build and verification

- [Android build](skills/android-build/SKILL.md) — debug, instrumentation, lint, and signed release builds.
- [iOS build](skills/ios-build/SKILL.md) — simulator/device builds and extension signing.
- [Device verification](skills/device-verification/SKILL.md) — physical/simulated runtime checks and evidence.

## Store delivery

- [Version and release](skills/version-release/SKILL.md) — versions, upload numbers, and release records.
- [Release preflight](skills/release-preflight/SKILL.md) — blockers before production builds.
- [Release artifacts](skills/release-artifacts/SKILL.md) — signed APK/AAB/archive/IPA validation.
- [Store submission content](skills/store-submission-content/SKILL.md) — localized, versioned store text and history.
- [ATT review](skills/att-review/SKILL.md) — Apple tracking disclosure, consent ordering, and review evidence.

## Additional workflows

- [Tiled face detection](skills/tiled-face-detection/SKILL.md) — high-resolution detection without losing coordinate accuracy (from Blurrf design documents).
- [Face detector selection](skills/face-detector-selection/SKILL.md) — native detector tradeoffs, coordinate contracts, and measured quality (from Blurrf design documents).
- [Offline data integrity](skills/offline-data-integrity/SKILL.md) — safe import/share/persistence and restoration (from Kappan design documents).

The eight migrated topics cover seven skills shared by both source projects, plus Blurrf's ATT review skill. The remaining workflows were extracted from their design and troubleshooting documents. Where source instructions diverge, follow the consumer repository's configuration instead of assuming either source project's scripts or version scheme.
