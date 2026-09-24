# App development skills

Reusable English-language mobile workflows. `skills/universal/` holds stack-neutral task contracts (use only when that domain applies); `skills/platform/` holds Android, iOS, Flutter, and Kotlin Multiplatform instructions. Each leaf has a stable frontmatter name and a `SKILL.md`; discover recursively with `skills/**/SKILL.md` or an equivalent loader. Select one starting skill by the task below, then follow its links when another domain is involved. Always inspect the consuming app's actual contracts and tooling.

## Route by task

Use a **platform** skill when the problem depends on its compiler, UI toolkit, build tool, or store console. Otherwise start with the applicable **universal** skill. For a store launch: version → preflight → artifacts → the target store's submission workflow; store copy is separate. Compilation, submission, review approval, and public availability are distinct outcomes.

## Universal workflows

### Device evidence

- **App builds but the changed path needs a real run:** [Device verification](skills/universal/build/device-verification/SKILL.md).

### Release preparation

- **User version or upload build number:** [Version and release](skills/universal/release/version-release/SKILL.md).
- **Blockers before producing a release:** [Release preflight](skills/universal/release/release-preflight/SKILL.md).
- **Signed mobile artifacts and metadata checks:** [Release artifacts](skills/universal/release/release-artifacts/SKILL.md), using only the targeted platform branch.
- **Localized store claims and submission history:** [Store submission content](skills/universal/release/store-submission-content/SKILL.md).

### Local data and collaboration

- **Offline document import, sharing, storage, or restoration:** [Offline data integrity](skills/universal/data/offline-data-integrity/SKILL.md).
- **Event replay, retries, or conflicting offline edits:** [Local-first event sync](skills/universal/data/local-first-event-sync/SKILL.md).
- **Shared resource authorization, invitations, or local/cloud cutover:** [Shared cloud access](skills/universal/data/shared-cloud-access/SKILL.md).

### Camera and vision

- **Camera gestures and asynchronous capture-session state:** [Camera gesture session](skills/universal/camera/camera-gesture-session/SKILL.md).
- **Tap-to-focus, metering, saved orientation, or mirroring:** [Camera focus and orientation](skills/universal/camera/camera-focus-orientation/SKILL.md).
- **Photo/video finalization or destination saving:** [Camera media output](skills/universal/camera/camera-media-output/SKILL.md).
- **Choose an on-device face detector for measured quality:** [Face detector selection](skills/universal/camera/face-detector-selection/SKILL.md).
- **Detect small faces in large images with crops:** [Tiled face detection](skills/universal/camera/tiled-face-detection/SKILL.md).

### Product experience

- **Improve friction in an existing user task:** [Mobile UX improvement](skills/universal/experience/mobile-ux-improvement/SKILL.md).
- **Audit screen states, accessibility, or adaptive layouts:** [Mobile UI/UX verification](skills/universal/experience/mobile-ui-ux-verification/SKILL.md).
- **Fold/flip posture or split-screen/iPad multiwindow state:** [Adaptive window and posture verification](skills/universal/experience/adaptive-window-posture/SKILL.md).
- **Tap/drag/scroll/pinch interactions collide:** [Mobile gesture interactions](skills/universal/experience/mobile-gesture-interactions/SKILL.md).
- **Translate app UI or change language preference:** [Mobile localization](skills/universal/experience/mobile-localization/SKILL.md); route store-only copy to submission content.

## Platform-specific workflows

### Android and Google Play

- **Android Gradle, SDK, build, or signing:** [Android build](skills/platform/android/android-build/SKILL.md).
- **Expensive or unnecessary Jetpack Compose recomposition:** [Compose recomposition](skills/platform/android/compose-recomposition/SKILL.md).
- **Play Console upload, review, rejection, or publication:** [Google Play submission](skills/platform/android/google-play-submission/SKILL.md).

### iOS and App Store

- **iOS Xcode, simulator/device build, or provisioning:** [iOS build](skills/platform/ios/ios-build/SKILL.md).
- **App Store Connect submission, App Review, or release:** [App Store submission](skills/platform/ios/app-store-submission/SKILL.md).
- **iOS tracking-consent review finding:** [ATT review](skills/platform/ios/att-review/SKILL.md).

### Flutter

- **Flutter Android/iOS build and signing:** [Flutter mobile build](skills/platform/flutter/flutter-mobile-build/SKILL.md).

### Kotlin Multiplatform

- **Shared Kotlin domain versus native Android/iOS boundaries:** [KMP platform boundaries](skills/platform/kmp/kmp-platform-boundaries/SKILL.md).

