---
name: release-artifacts
description: Produce and verify signed Android APK/AAB and iOS xcarchive/IPA with matching release metadata, checksums, and submission readiness.
---

# Release artifacts

## Before building

Pass [release-preflight](../release-preflight/SKILL.md). Lock intended source commit, version/build numbers, app IDs, distribution identities, and export method. Use the consumer project's scripts if supplied; Blurrf has `scripts/build-android-release.sh` and `scripts/prepare-ios-archive-dsyms.sh`, while Kappan does not have production Android signing configured. Keep output in ignored build storage, never in Git.

## Android

1. Build the intended release APK and AAB with the project's signed release tasks/scripts. Confirm exit status, actual artifact paths, and `validateSigningRelease`, bundle signing, and package tasks where applicable.
2. Inspect Android Gradle Plugin `output-metadata.json` and the merged release manifest for application ID, version name, and build number. Verify APK certificates using the SDK's `apksigner verify --print-certs '<APK>'`.
3. Verify the AAB signature with an appropriate JAR signature verifier (`jarsigner -verify -verbose -certs '<AAB>'`); the Gradle signing task succeeding alone is not an independent signature check. Only Play Console can establish final Play acceptance.

## iOS

1. Archive a generic iOS device Release build, adapting the project's project, scheme, and archive location:

```sh
xcodebuild archive -project '<PROJECT.xcodeproj>' -scheme '<SCHEME>' \
  -configuration Release -destination 'generic/platform=iOS' \
  -archivePath '<ARCHIVE.xcarchive>' -derivedDataPath '<DERIVED_DATA>'
```

2. Inspect archive `Info.plist` for bundle ID, `CFBundleShortVersionString`, `CFBundleVersion`, architecture, Team, and signing identity. If the project embeds third-party frameworks requiring separately prepared dSYMs, run its *documented* symbol-preparation script **after** archiving and verify each binary/dSYM UUID match before export; do not assume every vendor SDK has this requirement.
3. Export with a securely configured `ExportOptions.plist` for the intended store, using `xcodebuild -exportArchive -archivePath '<ARCHIVE.xcarchive>' -exportPath '<EXPORT_DIR>' -exportOptionsPlist '<OPTIONS.plist>'`. Where the project's version policy forbids automatic version rewriting, set `manageAppVersionAndBuildNumber=false`.
4. Verify the IPA and `DistributionSummary.plist`: store profile, distribution certificate, Team, version/build, architectures, and `get-task-allow=false`. A successful archive does not imply successful export. For symbol upload, use the project's documented archive-upload path if IPA transport omits dSYMs.

## Handoff

Check Android/iOS identifiers and version/build mapping against the release record. Calculate SHA-256 for each deliverable, store artifact path/size/hash/source commit/signature result in a release manifest, and verify checksums against the final files. Do not report unsigned, debug-signed, or export-failed files as production releases; record store validation separately from local verification.
