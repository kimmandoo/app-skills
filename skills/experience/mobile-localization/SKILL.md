---
name: mobile-localization
description: Implement or extend multilingual mobile UI in KMP, native Android/iOS, or Flutter while keeping language preference, OS resources, formatted copy, accessibility, and store listings consistent.
---

# Mobile localization

## Establish the language contract

1. Inspect the actual supported locales, UI string sources, settings storage, Android/iOS resource bundles, Flutter localization configuration if present, and store listing locales. Do not infer multilingual support merely from localization delegates or a single supported locale; decide the supported set from product requirements and available translations. Identify which screens, errors, notifications, widgets, and accessibility labels display app-owned text.
2. Use one app-language preference as the source of truth: an explicit supported-language override, or **follow system**. On launch, restore the persisted choice; resolve follow-system against the device/app-preferred locale order, then fall back deterministically to the product's supported fallback locale. Define how unknown, removed, or malformed saved choices fall back without silently overwriting an explicit valid choice. Keep the stored preference separate from the resolved locale so a system change can take effect when follow-system is selected.
3. Propagate the resolved locale through the whole app and its UI resource lookup, not just a settings screen. On a user change, persist the preference, rebuild/reconfigure affected views and platform formatters, and verify it survives process restart. On a system language change, update only when following system. Check that an explicit override remains stable across system changes.

## Wire each applicable client

- **KMP with native Android and iOS:** Share locale identifiers/selection semantics and app-owned copy where useful; adapt each native client's locale input, persistence, observable state, and resource lookup to the same contract. Do not assume a shared string table automatically localizes OS-owned dialogs, app metadata, or platform widgets.
- **Android/Compose:** Read the effective configuration locale(s) for follow-system and observe configuration changes. Connect the selected locale to all app-owned Compose text and Android resources consistently; if using Android per-app language APIs, reconcile those APIs with any in-app preference rather than creating competing authorities. Put translated app labels, notification text, and permission rationale copy in appropriate resources; audit configuration changes, activity recreation, and cached strings.
- **iOS/SwiftUI:** Resolve the system/app-preferred language to a supported language; persist an explicit choice or follow-system setting and publish changes so views refresh. Use localized app resources or a shared catalog consistently for app-owned text. Check language-sensitive dates/numbers and SwiftUI locale environment where applicable. A custom in-app override does not automatically change the language of OS permission prompts or other bundle-localized system UI.
- **Flutter:** First inspect `MaterialApp`/`CupertinoApp` locale, supported locales, delegates, app-owned strings, and locale state. `flutter_localizations` delegates with a single supported locale localize framework widgets only; they do not establish multilingual app copy. When multilingual support is requested, supply translated app resources/delegates for the actual supported set, resolve system fallback, persist an optional user override, and make the app rebuild with the resolved locale. Avoid pinning a fixed `locale` when follow-system should respond to changes. Confirm native Android/iOS bundle resources separately.

## Translate the right surfaces

- Keep app-owned screens, accessibility announcements, error messages, settings language names, and notifications in the app's localization pipeline. Audit launch, background, empty, error, settings, and permission-denied states, not only the happy path. Distinguish an app-authored permission explanation from the OS prompt itself.
- Localize iOS `InfoPlist.strings` usage descriptions and other displayed bundle metadata for every shipped language needed by the platform; ensure the built bundle actually contains them. Android manifest/resource labels and platform-facing rationale or notification copy have their own resource path. Never imply that changing runtime app strings changes an already-presented system prompt.
- Store titles, descriptions, screenshots, release notes, and privacy text live in store metadata, not the binary's in-app catalog. Coordinate changed claims and listing locales with [store-submission-content](../../release/store-submission-content/SKILL.md); compare final store copy to the shipped UI and actual behavior. Do not assume changing an in-app translation updates a store listing or vice versa.

## Format meaning, not fragments

- Use named or positional placeholders with translator context and verify arguments in every locale. Keep whole sentences translatable; do not concatenate translated fragments or embed fixed word order. Use locale-aware plural/select rules driven by numeric values rather than English suffixes or comparisons to preformatted numbers; validate zero, singular, plural, and any other categories required by each target language.
- Format numbers, dates, times, currencies, units, and lists with locale-aware APIs and the appropriate calendar/time zone; decide separately whether a value is localized display text or an invariant machine identifier. Preserve placeholders, escaping, links, and accessibility meaning through translation. Treat missing keys as a visible localization defect to fix, not as permission to ship mixed-language UI.

## Prove the shipped surfaces

1. Run the real app for each supported language and a device/app language not supported by the product. Check first launch in follow-system, changing system language while following, explicit override, changing system language under override, switching back to follow-system, and relaunch. Confirm app-owned UI and framework widgets agree where the platform allows; record intentional OS-prompt language independence.
2. Exercise an actual permission request on a fresh/reset authorization state and inspect the installed bundle's iOS usage descriptions or Android resources, as applicable. Verify any app-authored rationale separately. Compare localized store copy in the intended submission surface, not only in source files.
3. Exercise interpolation and plural boundaries, number/date display, errors, accessibility labels, RTL direction/mirroring where applicable, and large text/dynamic type/font scaling on real screens. Check wrapping, clipping, navigation, icons that should or should not mirror, keyboard/input direction, and screen-reader order. Capture the observed surface and unresolved translation or store-console limitations; source-text checks alone are not proof.
