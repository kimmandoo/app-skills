---
name: release-preflight
description: Check release scope, versions, store copy, privacy, signing, and assets before building or submitting mobile store binaries.
---

# Release preflight

## Inputs

Intended user version and build numbers for each target platform; source commit; target store version/track, release scope, and available signing/configuration. Read the project's version policy, release record, store listings, privacy declarations, and prior blockers. Distinguish unrelated working-tree changes from files owned by this release.

## Gate

1. Confirm the intended version classification under the project's own policy and that every upload build number increases relative to the last uploaded build on its store. For cross-platform releases, record the intended Android/iOS version and build-number mapping.
2. Match changelog and release scope against implemented, device-observable functionality. Prepare separate App Store and Google Play copy **only for the stores being targeted** via [store-submission-content](../store-submission-content/SKILL.md); compare locale claims, final build identifiers, and actual behavior.
3. Review listing changes, screenshots/icons (size, alpha, locale), age rating, account/payment disclosures, support/privacy-policy URLs, permissions, store privacy answers, and Data safety where applicable against the actual app and SDK behavior. When iOS tracking is involved, use [att-review](../../../ios/att-review/SKILL.md) for the privacy gate.
4. Verify required production signing identities, provisioning, keystore, and any production service/configuration used by the app are available through approved channels. Check presence and suitability, not secret contents in logs or documents.
5. Identify external console dependencies (consent messages, compliance declarations, store review data); distinguish a code/build pass from store-console completion.

## Stop and record

Block submission for mismatched metadata, reused upload number, unsupported claim, missing privacy/support URLs, inconsistent privacy declarations, invalid assets, unavailable signing/production configuration, or unclear ownership of existing user edits. Record pass/fail per gate, intended version/build, source commit, remaining blockers, and the next build/submission step. Only then run [release-artifacts](../release-artifacts/SKILL.md).
