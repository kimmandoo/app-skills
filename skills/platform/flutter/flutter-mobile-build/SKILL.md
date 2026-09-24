---
name: flutter-mobile-build
description: Use when building and verifying Flutter Android and iOS debug or release artifacts while distinguishing compilation from signing and store acceptance.
---

# Flutter mobile build

## Establish the project contract

From the consumer repository root, read `pubspec.yaml`, the Flutter/Dart SDK constraints, Android/iOS build settings, configured flavors, release signing, version source, and any documented build commands. Flutter commonly maps `pubspec.yaml`'s `version: <NAME>+<BUILD>` to Android `versionName`/`versionCode` and iOS `CFBundleShortVersionString`/`CFBundleVersion`; confirm whether this app overrides them. Read [version-release](../../../universal/release/version-release/SKILL.md) before choosing upload numbers. Never assume a particular app ID, scheme, track, or local config path.

## Debug and changed-path verification

1. Resolve dependencies with `flutter pub get`. Run `flutter analyze` and the relevant `flutter test` targets using the repository's supported options; fix actual failures rather than hiding analyzer rules.
2. For an Android debug build run `flutter build apk --debug` with the project's flavor/entrypoint flags if any. For iOS compilation on macOS, `flutter build ios --debug --no-codesign` is a **compile-only** check; it does not yield an installable device release.
3. Run `flutter run` on a selected emulator/physical device and exercise the changed user path (including navigation, permissions, async error states, and restart when persistence is involved). Follow [device-verification](../../../universal/build/device-verification/SKILL.md); do not report build success as feature success.

## Signed release

1. Pass [release-preflight](../../../universal/release/release-preflight/SKILL.md); check the configured Android upload key, iOS distribution certificates/profiles and capabilities, and any required production configuration. Pass build-time values through the project's documented ignored or CI-backed config without echoing secrets or committing them.
2. Build the intended Android deliverable using `flutter build appbundle --release` (and `flutter build apk --release` only if an APK is required). Build the iOS archive/IPA with `flutter build ipa --release` using the actual export/signing configuration. Do not treat an archive generated before a failed IPA export as a completed IPA.
3. Inspect actual output files, platform package/bundle IDs, version/build metadata, signing identities, and relevant entitlements. Independently verify package signatures and final checksums via [release-artifacts](../../../universal/release/release-artifacts/SKILL.md). Only store-console processing confirms upload acceptance.

## Stop conditions

Do not replace missing production keys, capabilities, or third-party config with debug values, claim an unsigned iOS build is installable, reuse an already uploaded build number, or report simulator-only checks as physical-device verification. Keep source, artifact metadata, store copy, and the recorded release state consistent.
