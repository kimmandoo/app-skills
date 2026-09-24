# App development skills

Reusable, English-language mobile workflows. Skills live at `skills/<category>/<skill>/SKILL.md`; their frontmatter names did not change. Discover them recursively (`skills/*/*/SKILL.md`, or the equivalent in your loader), then pick the narrowest matching workflow below. Read the consuming app's actual build, product, version, and signing contracts; no source project's defaults are assumed.

## Choose a starting skill

Match the task or failing stage to **one** entry below. For a store release, use version → preflight → signed artifacts → the **target store's** submission workflow; prepare store copy alongside the release and use ATT review only for tracking-specific iOS findings. A build, upload, review approval, and public availability are separate outcomes.

## Build and device

- **Android Gradle, SDK, build, or signing:** [Android build](skills/build/android-build/SKILL.md).
- **iOS Xcode, simulator/device build, or provisioning:** [iOS build](skills/build/ios-build/SKILL.md).
- **Flutter build for Android or iOS:** [Flutter mobile build](skills/build/flutter-mobile-build/SKILL.md).
- **App builds but changed behavior needs runtime proof:** [Device verification](skills/build/device-verification/SKILL.md).

## Release and store review

- **User version or upload build numbers:** [Version and release](skills/release/version-release/SKILL.md).
- **Release readiness before producing binaries:** [Release preflight](skills/release/release-preflight/SKILL.md).
- **Signed APK/AAB/archive/IPA creation and inspection:** [Release artifacts](skills/release/release-artifacts/SKILL.md).
- **Localized listing copy and submission history:** [Store submission content](skills/release/store-submission-content/SKILL.md).
- **Play Console upload, review, rejection, or publishing:** [Google Play submission](skills/release/google-play-submission/SKILL.md).
- **App Store Connect submission, App Review, or release:** [App Store submission](skills/release/app-store-submission/SKILL.md).
- **iOS tracking-consent review issue specifically:** [ATT review](skills/release/att-review/SKILL.md).

## Local data and collaboration

- **Offline document import, sharing, storage, or restoration:** [Offline data integrity](skills/data/offline-data-integrity/SKILL.md).
- **Offline edits, event replay, retries, or conflicting versions:** [Local-first event sync](skills/data/local-first-event-sync/SKILL.md).
- **Cloud membership, invitations, permissions, or local/cloud cutover:** [Shared cloud access](skills/data/shared-cloud-access/SKILL.md).

## Camera and vision

- **Camera tap/hold gestures or asynchronous recording state:** [Camera gesture session](skills/camera/camera-gesture-session/SKILL.md).
- **Tap-to-focus, metering, image rotation, or mirroring:** [Camera focus and orientation](skills/camera/camera-focus-orientation/SKILL.md).
- **Photo/video capture finalization or media-library saving:** [Camera media output](skills/camera/camera-media-output/SKILL.md).
- **Choose and benchmark an on-device face detector:** [Face detector selection](skills/camera/face-detector-selection/SKILL.md).
- **Detect small faces in large images using overlapping crops:** [Tiled face detection](skills/camera/tiled-face-detection/SKILL.md).

## Product experience

- **Improve friction in an existing user task:** [Mobile UX improvement](skills/experience/mobile-ux-improvement/SKILL.md).
- **Audit screen states, accessibility, or adaptive layouts:** [Mobile UI/UX verification](skills/experience/mobile-ui-ux-verification/SKILL.md).
- **Tap/drag/scroll/pinch actions collide:** [Mobile gesture interactions](skills/experience/mobile-gesture-interactions/SKILL.md).
- **Translate in-app UI or change language preference:** [Mobile localization](skills/experience/mobile-localization/SKILL.md); use store submission content for store-only text.

## Architecture

- **Share domain logic across native Android and iOS:** [KMP platform boundaries](skills/architecture/kmp-platform-boundaries/SKILL.md).

