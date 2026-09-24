# Work checkpoint

## 2026-09-24 — top-level platform skill folders

- Status: complete. Preserved the user's relocation of eight unchanged platform skills from `skills/platform/{android,ios,flutter,kmp}/` to `skills/{android,ios,flutter,kmp}/`. Kept 18 universal skills under `skills/universal/`, stable skill names, and the UX attribution/license.
- Updated the eight platform catalog routes, fifteen platform-skill links to universal workflows, and the universal release-preflight link to iOS ATT. Prior checkpoint sections describe the historical layouts at those checkpoints, not current discovery paths.
- Verification: JavaScript recursive discovery found 26 unique one-to-one frontmatter/catalog routes (18 universal, Android 3, iOS 3, Flutter 1, KMP 1), with all 63 relative links resolved. Before repair, the user's move exposed 24 broken links. Git blob hashes confirmed the eight relocations initially preserved file content; no consuming app or device was run for this documentation-only path change.
- Changed files: eight relocated platform skill files, `skills/universal/release/release-preflight/SKILL.md`, `README.md`, `CHANGELOG.md`, and this checkpoint. User-owned untracked files were left alone. Blockers: none; next action: none.

## 2026-09-24 — universal/platform routing, Compose efficiency, adaptive devices, history naming

- Status: complete. Split 26 recursively discoverable skills into `skills/universal/` (18) and `skills/platform/` (8); retained stable frontmatter names and moved the MIT notice with the UX skill. Generalized reusable camera, data, localization, device-verification, and experience instructions without removing domain safeguards.
- Added `skills/platform/android/compose-recomposition/SKILL.md` for measured Android recomposition changes and `skills/universal/experience/adaptive-window-posture/SKILL.md` adapted from the read-only Kappan `skills/adaptive-screens.md` for real Android fold/flip/split-screen and supported iPad window/scene transitions. Removed project-specific EPUB, bundle ID, and fixed posture IDs; preserved actual task state, safe data, hinge/keyboard avoidance, independent-scene checks, and honest hardware limits.
- Updated `README.md` routes and all moved cross-category links; reworded the source-named camera extraction commit message without changing committed content or author. The local rewritten history was not pushed.
- Verification: recursive JavaScript smoke found 26 unique English-frontmatter skills, 18 universal and 8 platform, 26 one-to-one catalog routes, and 63 resolved relative links; checked distinct Compose, KMP, localization, camera, both store submissions, and fold/flip/multiwindow routes, plus posture/state, camera-safety, and Compose correctness gates. No consuming app, camera, store console, physical foldable, iPad, or profiling device was accessed to apply these documentation workflows.
- Changed files: relocated skill files and UX license, focused reusable-skill polish, new Compose and adaptive-window skills, `README.md`, `CHANGELOG.md`, and this checkpoint. User-owned untracked files in both repositories were not touched. Blockers: none; next action: none.

## 2026-09-24 — categorized routing and store submission

- Status: complete. Moved the 22 existing skills into `skills/{build,release,data,camera,experience,architecture}/<skill>/`, preserved their frontmatter names and the upstream MIT notice, and replaced the duplicated catalog/routes with 24 single-entry task routes in `README.md`.
- Added distinct `google-play-submission` and `app-store-submission` skills under `skills/release/` for Console upload/submission, review-status decisions, rejection/resubmission, and publication checks. Build verification, preflight, store copy, and ATT remain separate linked workflows. Based on official Google Play Console Help and Apple App Store Connect Help read without changing source apps.
- Verification: recursive JavaScript discovery checked 24 unique English-frontmatter skills, six categories, one catalog route per skill, 56 resolved relative links, representative Google Play/App Store review-status routes, and preserved license notice. A targeted search found no remaining references to the former flat skill paths. No store Console or app/device was accessed; these documentation workflows were not used to publish an app.
- Changed files: 22 relocated skill files, the relocated `UPSTREAM_LICENSE`, cross-category references in four existing skills, two new store skills, `README.md`, `CHANGELOG.md`, and this checkpoint. User-owned untracked files were not touched. Blockers: none; next action: none.

## 2026-09-24 — mobile UX improvement skill

- Status: complete. Added the UX workflow, now at `skills/universal/experience/mobile-ux-improvement/SKILL.md`, as a focused diagnose/change/compare guide for mobile task friction, truthful feedback, and restrained motion; catalog listed 22 skills at that checkpoint.
- Source and license: adapted the MIT-licensed methods in `emilkowalski/skills` for existing native/Kotlin Multiplatform and Flutter workflows, without copying web/Expo-specific recipes. Preserved upstream copyright and full MIT notice, now at `skills/universal/experience/mobile-ux-improvement/UPSTREAM_LICENSE`; upstream was accessed read only.
- Verification: JavaScript discovery smoke resolved all 47 relative skill/catalog links, matched 22 skill folders to names and catalog entries, routed an existing-mobile-task improvement request to the new skill, checked an offline-save and high-frequency interaction scenario, and found attribution and license notice. This documentation-only change was not tested on a running app; actual UX gains must be checked when applying the skill.
- Changed files at that checkpoint: `README.md`, `CHANGELOG.md`, this checkpoint, and two UX files now under `skills/universal/experience/mobile-ux-improvement/`. Blockers: none; next action: none.

## 2026-09-24 — portable skill expansion and policy cleanup

- Status: complete. Added `flutter-mobile-build`, `local-first-event-sync`, `shared-cloud-access`, `kmp-platform-boundaries`, `mobile-ui-ux-verification`, `mobile-gesture-interactions`, and `mobile-localization`; indexed 21 skills.
- Portability: removed source project names, repository-specific paths, version/tag conventions, fixed detector tuning, and dual-store assumptions from the skill bodies/catalog. Kept domain-specific technical APIs conditional on the consumer's actual architecture.
- Camera policy: documented explicit consent, visual and/or audible indication, device-enforced cues, and platform policy checks. The source camera app was not edited.
- Sources: `../gguldong` had seven pre-existing modified files. Its Git status and SHA-256 hashes for those files were identical after extraction. The camera source repository remained clean; other referenced projects were read only.
- Verification: JavaScript smoke checked 21 English frontmatter/catalog entries, 43 relative links, all code fences, banned source identifiers, the camera-consent route, optional checksum semantics, and localization fallback. No project builds were run for documentation-only changes.
- Changed files: `README.md`, `CHANGELOG.md`, this checkpoint, edited prior `skills/*/SKILL.md` files, and seven new skill folders. Blockers: none; next action: none.

## 2026-09-24 — camera workflow extraction

- Status: complete. Added `camera-gesture-session`, `camera-focus-orientation`, and `camera-media-output`; indexed all fourteen skills in `README.md`.
- Source repository: the camera project was read only. Git reported no tracked/untracked changes afterward; hashes of the consulted source files matched the initial inventory.
- Verification: JavaScript discovery smoke checked fourteen named skills, catalog coverage, English text, balanced fences, 27 resolved internal links, and routing of gesture, focus, and save/finalization guidance. No camera hardware was available or needed to verify documentation-only changes.
- Changed files: `README.md`, `CHANGELOG.md`, this checkpoint, and three new `skills/*/SKILL.md` files.
- Blockers: none. Next action: none.

## 2026-09-24

- Status: complete. Migrated eight source skill topics to `skills/<topic>/SKILL.md`; extracted three additional skills and indexed all eleven in `README.md`.
- Source repositories: `../blurrf` and `../kappan` were read only; the source skill-file SHA-256 hashes matched the starting inventory after the copy.
- Verification: JavaScript discovery smoke checked eleven skill frontmatter names, English body text, README coverage, balanced code fences, and 22 resolved internal links. Source skill SHA-256 checks completed for both projects. No builds were run because these changes are documentation workflows only.
- Changed files: `README.md`, `CHANGELOG.md`, this checkpoint, and eleven `skills/*/SKILL.md` files.
- Blockers: none. Next action: maintain each skill when the corresponding project workflow changes; no continuation step is pending.
