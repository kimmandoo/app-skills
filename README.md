# App development skills

Reusable, English-language workflows for mobile application development. Each folder contains a `SKILL.md` with a trigger description, prerequisites, execution steps, and checks. Read the consuming repository's build files, version policy, product contract, and signing configuration before applying a skill; no source project's paths, defaults, or release rules are assumed.

## Build and verification

- [Android build](skills/android-build/SKILL.md) — debug, instrumentation, lint, and signed release builds.
- [iOS build](skills/ios-build/SKILL.md) — simulator/device builds and extension signing.
- [Flutter mobile build](skills/flutter-mobile-build/SKILL.md) — debug checks and signed Android/iOS Flutter releases.
- [Device verification](skills/device-verification/SKILL.md) — physical/simulated runtime checks and evidence.

## Store delivery

- [Version and release](skills/version-release/SKILL.md) — versions, upload numbers, and release records.
- [Release preflight](skills/release-preflight/SKILL.md) — blockers before production builds.
- [Release artifacts](skills/release-artifacts/SKILL.md) — signed APK/AAB/archive/IPA validation.
- [Store submission content](skills/store-submission-content/SKILL.md) — localized, versioned store text and history.
- [ATT review](skills/att-review/SKILL.md) — Apple tracking disclosure, consent ordering, and review evidence.

## Additional workflows

- [Tiled face detection](skills/tiled-face-detection/SKILL.md) — high-resolution detection with coordinate restoration and duplicate merging.
- [Face detector selection](skills/face-detector-selection/SKILL.md) — detector tradeoffs, coordinate contracts, and measured quality.
- [Offline data integrity](skills/offline-data-integrity/SKILL.md) — safe import/share/persistence and restoration.

## Offline collaboration

- [Local-first event sync](skills/local-first-event-sync/SKILL.md) — event validation, idempotent retry, and conflict handling.
- [Shared cloud access](skills/shared-cloud-access/SKILL.md) — role enforcement, safe cutover, and independent local copies.

## Camera workflows

- [Camera gesture session](skills/camera-gesture-session/SKILL.md) — asynchronous gesture/state policy and lifecycle-safe recording.
- [Camera focus and orientation](skills/camera-focus-orientation/SKILL.md) — tap-to-focus convergence and single-pass image rotation/mirroring.
- [Camera media output](skills/camera-media-output/SKILL.md) — consented capture, clear recording indication, and durable media-library save.

## Architecture and product experience

- [KMP platform boundaries](skills/kmp-platform-boundaries/SKILL.md) — shared-domain interfaces and native adapters.
- [Mobile UI/UX verification](skills/mobile-ui-ux-verification/SKILL.md) — adaptive screens, truthful states, accessibility, and real-surface checks.
- [Mobile gesture interactions](skills/mobile-gesture-interactions/SKILL.md) — tap/drag/pinch/scroll precedence and accessible alternatives.
- [Mobile localization](skills/mobile-localization/SKILL.md) — language preference, resources, formatting, and store metadata.

