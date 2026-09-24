---
name: version-release
description: Coordinate mobile app version and upload build-number changes with the targeted platform artifacts, store copy, and release records.
---

# Version and release

## Establish the version contract

Read the consumer project's version policy and determine its **actual** version source (for example, Gradle/Xcode settings, a shared properties file, or a Flutter manifest). Map each platform's user-visible version and upload build number back to that source. Do not introduce a second version authority without migrating the existing one.

1. Classify the release using the consumer project's version policy (for example patch/minor/major when it uses semantic versioning).
2. Before an upload, compare each platform's build number against its last uploaded build and choose a strictly higher accepted number. A local rebuild need not consume an upload number; a rejected build uploaded already does.
3. Update the configured version source(s), changelog, and per-store content via [store-submission-content](../store-submission-content/SKILL.md).
4. Run [release-preflight](../release-preflight/SKILL.md) before [release-artifacts](../release-artifacts/SKILL.md). Check metadata on each targeted platform against the intended version/build mapping; if platform upload numbers differ, record the distinction instead of reporting parity.
5. Record source commit, artifact names/hashes, upload numbers, destinations, submission state, and any blockers in the project's release record.

## Git release automation

Inspect the repository's CI triggers before creating commits/tags. If automated releases require a special commit subject, tag pattern, or both, confirm the exact documented gate and point the tag at the intended release commit. Do not apply another repository's tag or commit convention.

## Stop conditions

Do not upload a reused build number, publish store copy for missing functionality, or label a partially signed/unverified package a release. A version update alone does not prove store acceptance.
