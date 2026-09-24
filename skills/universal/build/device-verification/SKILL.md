---
name: device-verification
description: Prove changed mobile behavior on an installed app using the target device and tooling, rather than inferring success from compilation or installation.
---

# Device verification

## Setup

Identify the changed screen/gesture, original bug reproduction, required permissions, and expected visible result. Use disposable test data and a test account if applicable. Preserve existing device and user data; do not wipe a device just to simplify testing. Record the exact build, device/OS, and whether it is physical or simulated. Choose a physical device or emulator/simulator that supports the feature under test; identify the app's actual installable artifact, application identifier, and supported install/launch tooling for that target.

## Install, launch, and observe

1. Use the target's supported toolchain to list/select the intended device, install the matching build, stop an already-running instance if a clean launch is needed, and launch the installed app. Verify that the installed build, not an old installation or a test artifact, is what opens. Do not assume every app ships both Android and iOS clients, or that its framework uses the same build output as another stack.
2. Select commands **only for the target that exists**. For example, an Android APK may be installed with `adb -s '<SERIAL>' install -r '<APK>'` and launched with `adb -s '<SERIAL>' shell monkey -p '<APPLICATION_ID>' -c android.intent.category.LAUNCHER 1`; a signed iOS device app may use `xcrun devicectl device install app --device '<UDID>' '<DEVICE_APP>'` and `xcrun devicectl device process launch --device '<UDID>' '<BUNDLE_ID>'`; an iOS simulator app may use `xcrun simctl install '<SIMULATOR_UDID>' '<SIMULATOR_APP>'` and `xcrun simctl launch '<SIMULATOR_UDID>' '<BUNDLE_ID>'`. Discover device IDs with the toolchain and use its equivalent commands for other platforms or app formats. Do not pass a simulator artifact to a physical device or vice versa.
3. Capture the actual surface and relevant logs through the device, simulator/emulator, or app tooling. If instrumentation covers the changed behavior, run the configured tests with their matching artifacts; installing a test package alone is not running it. For share extensions or other OS-launched components, check the required signing/entitlements and exercise the real system entry point before interpreting failure as an app bug.

## Scenario checks

1. Navigate to the affected surface and perform the real gesture/button/file/permission sequence. Inspect loading, success, error, cancellation, and retry states that matter to the change.
2. For a bug, repeat its known failure scenario after the fix; do not rerun an already user-confirmed failure merely to dispute it.
3. For a feature that imports, exports, or persists data, use fixtures, close and relaunch the app, and verify the relevant content and state. Confirm cancellation or duplicate input does not publish partial data. Where the platform restores windows/scenes, test termination immediately after an update; restored snapshots may differ from the most recently persisted selection. Where multiwindow is supported, restore each window independently.
4. Keep screenshots/logs as evidence with device, OS, build, and the exact steps exercised. Report launch-only, simulator-only, or build-only checks as such; never claim physical-device validation without a physical device.

For fold/flip posture and independently resized multiwindow scenarios, follow [adaptive-window-posture](../../experience/adaptive-window-posture/SKILL.md).

Separate device lock, permission prompts, missing signing profiles, and unavailable device runtimes from application behavior.
