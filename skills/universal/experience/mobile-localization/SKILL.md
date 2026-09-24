---
name: mobile-localization
description: Implement or extend multilingual mobile UI across mobile stacks while keeping language preference, displayed copy, formatting, accessibility, and applicable platform and store metadata consistent.
---

# Mobile localization

## Establish the language contract

1. Inspect the actual supported locales, app-owned string sources, settings storage, framework and platform resource configuration where present, and store listing locales if published. Do not infer multilingual support merely from installed localization machinery or a single supported locale; decide the supported set from product requirements and available translations. Identify which screens, errors, notifications, widgets, and accessibility labels display app-owned text.
2. Use one app-language preference as the source of truth: an explicit supported-language override, or **follow system**. On launch, restore the persisted choice; resolve follow-system against the device/app-preferred locale order, then fall back deterministically to the product's supported fallback locale. Define how unknown, removed, or malformed saved choices fall back without silently overwriting an explicit valid choice. Keep the stored preference separate from the resolved locale so a system change can take effect when follow-system is selected.
3. Propagate the resolved locale through the whole app and its UI resource lookup, not just a settings screen. On a user change, persist the preference, rebuild/reconfigure affected views and platform formatters, and verify it survives process restart. On a system language change, update only when following system. Check that an explicit override remains stable across system changes.

## Connect the applicable locale adapters

1. Identify who owns the language preference, how the running UI observes its resolved value, and where each app-owned string is looked up. Map each shipped client or module to that contract; share selection semantics across clients without assuming they share a resource catalog. Avoid competing authorities between an in-app setting and an OS per-app language setting, if used. A framework's built-in widget translations alone do not translate the product's own copy.
2. Adapt the actual UI framework's locale input and resource lookup to the resolved locale. Make a preference change refresh mounted screens, cached strings, accessibility text, and locale-dependent formatters; make follow-system observe device/app language changes rather than pinning a fixed locale. Check view recreation and restoration so old text does not reappear. For example, an Android client may need to reconcile per-app language APIs and configuration changes; a SwiftUI client may need to publish locale changes to its environment; a Flutter client may need to update its app-level locale and delegates. Use only the adapters that exist in the product.
3. Identify which text comes from the app, the UI framework, the installed platform bundle, or the OS itself. Supply translated resources for each applicable owner and shipped language; verify that the packaged resources and declared supported locales match the UI contract. In-app runtime overrides do not necessarily change OS-owned dialogs or bundle-localized metadata. Check notification and permission-explanation paths separately from ordinary screens.

## Translate the right surfaces

- Keep app-owned screens, accessibility announcements, error messages, settings language names, and notifications in the app's localization pipeline. Audit launch, background, empty, error, settings, and permission-denied states where applicable, not only the happy path.
- Localize displayed platform-bundle metadata and permission usage descriptions for every shipped language required by the target platform, and inspect the installed package to confirm their presence. For example, iOS usage descriptions may use `InfoPlist.strings`, while Android labels and notification text may use platform resources. Distinguish app-authored permission explanations from OS prompts; never imply that changing runtime app strings changes an already-presented system prompt.
- If the app is listed in a store, its titles, descriptions, screenshots, release notes, and privacy text live in store metadata, not the binary's in-app catalog. Coordinate changed claims and listing locales with [store-submission-content](../../release/store-submission-content/SKILL.md); compare final store copy to the shipped UI and actual behavior. Do not assume changing an in-app translation updates a store listing or vice versa.

## Format meaning, not fragments

- Use named or positional placeholders with translator context and verify arguments in every locale. Keep whole sentences translatable; do not concatenate translated fragments or embed fixed word order. Use locale-aware plural/select rules driven by numeric values rather than English suffixes or comparisons to preformatted numbers; validate zero, singular, plural, and any other categories required by each target language.
- Format numbers, dates, times, currencies, units, and lists with locale-aware APIs and the appropriate calendar/time zone; decide separately whether a value is localized display text or an invariant machine identifier. Preserve placeholders, escaping, links, and accessibility meaning through translation. Treat missing keys as a visible localization defect to fix, not as permission to ship mixed-language UI.

## Prove the shipped surfaces

1. Run the real app for each supported language and a device/app language not supported by the product. Check first launch in follow-system, changing system language while following, explicit override, changing system language under override, switching back to follow-system, and relaunch. Confirm app-owned UI and framework widgets agree where the platform allows; record intentional OS-prompt language independence.
2. Exercise an actual permission request on a fresh/reset authorization state where the app requests permission; inspect the installed platform resources or bundle descriptions as applicable, and verify any app-authored rationale separately. If store listings exist, compare localized copy in the intended submission surface, not only in source files.
3. Exercise interpolation and plural boundaries, number/date display, errors, accessibility labels, RTL direction/mirroring where applicable, and large text/dynamic type/font scaling on real screens. Check wrapping, clipping, navigation, icons that should or should not mirror, keyboard/input direction, and screen-reader order. Capture the observed surface and unresolved translation or store-console limitations; source-text checks alone are not proof.
