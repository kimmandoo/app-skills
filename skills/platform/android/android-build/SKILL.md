---
name: android-build
description: Build and verify an Android debug APK or a signed release APK/AAB; use when preparing Android binaries or diagnosing Gradle, SDK, or signing failures.
---

# Android build

## Inputs and prerequisites

- Work from the app repository root. Read its Gradle module, SDK/JDK toolchain, version source, build variants, and any repository build scripts before choosing commands.
- Configure the Android SDK via `ANDROID_HOME` or the repository's ignored `local.properties`; use the project's required JDK.
- For release, obtain the configured keystore and production configuration through the project's secure channels. Never invent keys, advertising IDs, or placeholder release values.

## Debug

1. Prefer the repository's supported build script. Otherwise identify the app module and run `./gradlew :<APP_MODULE>:assembleDebug :<APP_MODULE>:assembleDebugAndroidTest :<APP_MODULE>:lintDebug --console=plain` when those tasks exist.
2. Check exit status and the actual output paths under the module's `build/outputs/`. Inspect the lint report and run the built instrumentation APK on a device when applicable; a successful build is not a successful behavioral test.
3. Use [device-verification](../../../universal/build/device-verification/SKILL.md) to exercise the changed flow, including permission, file-import, and rotation paths where relevant.

## Release

1. Check the release variant's signing configuration and version source. If production signing is unavailable, stop rather than label an unsigned artifact distributable. Use a repository release script only when one is documented and maintained there.
2. Build the project's APK and/or AAB using its actual tasks or script. Locate the outputs, verify Gradle signing tasks, and inspect metadata and certificates as described in [release-artifacts](../../../universal/release/release-artifacts/SKILL.md).
3. Distinguish an installable signed APK from an uploadable signed AAB; passing `assembleRelease` alone proves neither store acceptance nor on-device behavior.

## Failure handling

Record the first relevant error, task exit status, artifacts that exist, and whether the failure is an environment, provisioning, signing, lint, test, or app-code failure. Correct the cause and rerun the same build. Do not bypass release signing to claim success.

