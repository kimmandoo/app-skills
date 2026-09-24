---
name: ios-build
description: Build an iOS simulator app or signed device app and distinguish compile, code-signing, provisioning, and installation results.
---

# iOS build

## Prerequisites

Use macOS with the project's supported Xcode and dependencies. Identify the actual `.xcodeproj`/`.xcworkspace`, scheme, configuration, version source, and app extensions before running commands. A device build needs a paired device in Developer Mode, signing identity, Team, and profiles for the app and every extension. Keep certificates, keys, and provisioning files out of source control.

## Simulator

Use the repository's debug script if one exists; otherwise adapt:

```sh
xcodebuild -project '<PROJECT.xcodeproj>' -scheme '<SCHEME>' \
  -configuration Debug -destination 'generic/platform=iOS Simulator' \
  -derivedDataPath '<DERIVED_DATA>' build
```

For targets with App Groups or share extensions, preserve the signing/entitlement behavior required by the simulator; disabling signing may check compilation but does not prove the extension works. Confirm exit code 0, `** BUILD SUCCEEDED **`, and the `.app` under `Build/Products/Debug-iphonesimulator/`.

## Device

1. Check `xcrun devicectl list devices` and `security find-identity -v -p codesigning`. Confirm the selected device, configured Team, and the app/extension App IDs.
2. If an App Group is used, register it with the developer account, attach it to **both** app and extension IDs, and ensure **both** profiles include the entitlement. `-allowProvisioningUpdates` does not register a missing group by itself.
3. Use the repository's signed device build script if available, or adapt:

```sh
xcodebuild -project '<PROJECT.xcodeproj>' -scheme '<SCHEME>' \
  -configuration Debug -destination 'id=<DEVICE_UDID>' \
  -derivedDataPath '<DERIVED_DATA>' -allowProvisioningUpdates build
```

4. Verify signing of the app and embedded extensions and check the device `.app` output under `Build/Products/Debug-iphoneos/`. Install, launch, and exercise the changed behavior with [device-verification](../device-verification/SKILL.md). A simulator build, unsigned device build, or compile-only result is not a signed device build.

## Failure handling

Separate package resolution, Swift compilation, certificate/profile, entitlement, locked device, and app runtime errors. Report the exact stage reached; do not remove an extension or entitlement to turn a failed device build green. For store archives and exports, follow [release-artifacts](../release-artifacts/SKILL.md).
