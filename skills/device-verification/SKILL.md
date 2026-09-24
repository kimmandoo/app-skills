---
name: device-verification
description: Prove changed Android or iOS behavior on an installed app, rather than inferring success from compilation or installation.
---

# Device verification

## Setup

Identify the changed screen/gesture, original bug reproduction, required permissions, and expected visible result. Use a test account and non-sensitive fixture files. Preserve existing emulator and user data; do not wipe a device just to simplify testing. Record the exact build, device/OS, and whether it is physical or simulated.

## Android

```sh
adb devices
adb -s '<SERIAL>' install -r '<DEBUG_APK>'
adb -s '<SERIAL>' shell am force-stop '<APPLICATION_ID>'
adb -s '<SERIAL>' shell monkey -p '<APPLICATION_ID>' -c android.intent.category.LAUNCHER 1
```

If changed behavior is covered by instrumentation, install the matching `androidTest` APK and run its configured runner; do not mistake test installation for a test run.

## iOS

For a signed physical-device build:

```sh
xcrun devicectl list devices
xcrun devicectl device install app --device '<UDID>' '<DEVICE_APP>'
xcrun devicectl device process launch --device '<UDID>' '<BUNDLE_ID>'
```

For a simulator build:

```sh
xcrun simctl list devices booted
xcrun simctl install '<SIMULATOR_UDID>' '<SIMULATOR_APP>'
xcrun simctl launch '<SIMULATOR_UDID>' '<BUNDLE_ID>'
xcrun simctl io '<SIMULATOR_UDID>' screenshot '<SCREENSHOT_PATH>'
```

Check signing and App Group entitlements for share extensions before interpreting a failed share action as an app bug. Use the actual system share sheet to test share extensions, not only direct entry points.

## Scenario checks

1. Navigate to the affected surface and perform the real gesture/button/file/permission sequence. Inspect loading, success, error, cancellation, and retry states that matter to the change.
2. For a bug, repeat its known failure scenario after the fix; do not rerun an already user-confirmed failure merely to dispute it.
3. For persistence or import/export, use fixture files, close and relaunch the app, verify restored content and position, and confirm cancellation or duplicate input does not publish partial data. On iOS, test termination immediately after an update; scene snapshots need not match the most recently persisted selection. Where multiwindow is supported, restore each scene independently.
4. Keep screenshots/logs as evidence with device, OS, build, and the exact steps exercised. Report launch-only, simulator-only, or build-only checks as such; never claim physical-device validation without a physical device.

Separate device lock, permission prompts, missing profiles, and missing simulator runtimes from application behavior.
