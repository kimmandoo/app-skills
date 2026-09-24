---
name: version-release
description: Coordinate app version and build-number changes across Android, iOS, release copy, artifacts, and release records.
---

# Version and release

## Establish the version contract

Read the consumer project's version policy and determine its **actual** version source. Blurrf uses `version.properties` with `VERSION_NAME` (Android `versionName` / iOS `MARKETING_VERSION`) and `VERSION_CODE` (Android `versionCode` / iOS `CURRENT_PROJECT_VERSION`); Kappan instead configures each platform in its build files. Do not introduce a second version authority without migrating the existing one.

1. Choose patch/minor/major from user-visible compatibility changes and the project's policy.
2. Before an upload, compare each platform's build number against its last uploaded build and choose a strictly higher accepted number. A local rebuild need not consume an upload number; a rejected build uploaded already does.
3. Update the configured version source(s), changelog, and per-store content via [store-submission-content](../store-submission-content/SKILL.md).
4. Run [release-preflight](../release-preflight/SKILL.md) before [release-artifacts](../release-artifacts/SKILL.md). Check both platforms' artifact metadata against the intended version/build. If platform build numbers intentionally differ, explicitly record the mapping; do not falsely report parity.
5. Record source commit, artifact names/hashes, upload numbers, destinations, submission state, and any blockers in the project's release record.

## Git release automation

Inspect the repository's CI triggers before creating commits/tags. The Blurrf convention requires both `release(vX.Y.Z): ...` as the release commit subject and `release-vX.Y.Z` pointing at that commit; ordinary work must not impersonate a release commit. Treat that as an example, not a universal GitHub Actions rule.

## Stop conditions

Do not upload a reused build number, publish store copy for missing functionality, or label a partially signed/unverified package a release. A version update alone does not prove both stores accepted the binaries.
