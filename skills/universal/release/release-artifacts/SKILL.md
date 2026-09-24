---
name: release-artifacts
description: Produce and verify signed Android APK/AAB and/or iOS xcarchive/IPA for the intended platforms with correct release metadata, checksums, and submission readiness.
---

# Release artifacts

## Before building

Pass [release-preflight](../release-preflight/SKILL.md). Lock intended source commit, version/build numbers, app IDs, distribution identities, and export method. Use only the consuming project's configured build scripts and signing settings. Keep output in ignored build storage, never in Git.

Use only the target-platform sections below. Flutter, KMP, and native projects may produce the same distribution formats through different documented commands; apply the artifact/signature checks to the actual output, not to an assumed build system.

## Android

1. Build the intended signed Android release deliverables (APK, AAB, or both) with the project's configured tasks/scripts. Confirm exit status, actual artifact paths, and relevant signing/package tasks.
2. Inspect Android Gradle Plugin output metadata and the merged release manifest for application ID, version name, and build number. For an APK, verify certificates using the SDK's `apksigner verify --print-certs '<APK>'`.
3. For an AAB, verify its signature independently with an appropriate JAR signature verifier (`jarsigner -verify -verbose -certs '<AAB>'`). Only Play Console can establish final Play acceptance.

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

Check identifiers and version/build mapping for every platform being released against its release record. Calculate SHA-256 for each deliverable, store artifact path/size/hash/source commit/signature result in a release manifest, and verify checksums against the final files. Do not report unsigned, debug-signed, or export-failed files as production releases; record store validation separately from local verification.
