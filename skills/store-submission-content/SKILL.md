---
name: store-submission-content
description: Prepare version-specific App Store and Google Play release copy with localized claims, privacy updates, and an auditable submission history.
---

# Store submission content

## Inputs and output

Read the actual version source, last public version, changelog, implemented release behavior, base listings, privacy declarations, and store-console limits. Maintain separate version-scoped App Store and Google Play records (for example `store-assets/releases/<VERSION>/app-store.md` and `google-play.md` if the project uses that layout). Keep base listing unchanged unless the listing itself changes; a version record contains only overrides and submission history.

## Draft

1. Identify observable user changes since the previous public version. Do not advertise unimplemented features, internal symbols, refactors, SDK upgrades, or unverified guarantees.
2. For App Store, record localized What's New where applicable, promotional/listing overrides, review-note changes, screenshot changes, release method, build, and submission history. First-version What's New may be unavailable; record that instead of fabricating copy.
3. For Google Play, record localized release name/notes, listing and graphics overrides, target track/rollout, build, and submission history.
4. Cover every locale supported by the product and reconcile meanings across translations. The source example uses `ko-KR`, `en-US`, `ja-JP`; other products may have different locales. Keep user-visible claims consistent but store-specific wording separate.
5. Reassess App Privacy and Data safety whenever permissions, SDKs, ads, accounts, payment, or data handling change. Check actual platform character/field limits before submission.

## Re-submission and review

For a build-only retry at the same user version, update the build identifier and append the rejection/cancellation/new-submission record; do not create a fake new user version or change unchanged feature notes. Never erase earlier review history. Compare final copy against the shipped binary, version metadata, screenshot set, and real App Store Connect/Play Console state. If behavior or translation cannot be verified, remove the claim and mark it blocked.
