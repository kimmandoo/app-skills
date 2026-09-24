---
name: mobile-ui-ux-verification
description: Use when designing or verifying mobile screens, adaptive layouts, gestures, accessibility, and user-visible loading, empty, error, or offline states on actual devices.
---

# Verify mobile UI and UX on the real surface

## Define the experience before arranging controls

1. Identify the supported platforms, actual primary user tasks, entry and exit routes, data ownership, and which actions are destructive or unavailable without permission or connectivity. Map each task through success, cancellation, loading, empty, failure, and offline paths where applicable; add conflict and read-only paths only when the product supports concurrent changes or permissions. Do not invent accounts, synchronization, camera capture, or other product features.
2. Preserve the product's established visual language and use platform-appropriate controls, system back behavior, sheets, menus, pickers, and accessibility semantics. Keep the primary action discoverable, secondary status informative rather than obstructive, and destructive exits explicit when unsaved work would be lost. Use product-approved copy and assets rather than prescribing a logo, palette, layout, or fixed screen flow.
3. Lay out from the current *window and container* bounds, not physical device dimensions. Respect safe areas, system gesture edges, keyboard and system bars; keep controls reachable in compact windows, rotation, split-screen, and live resize. Let explanatory text wrap, important actions remain visible or scroll into view, and titles truncate without hiding meaning. Preserve the current task and meaningful position across reflow; do not persist transient zero-size geometry over valid state.

## Specify gesture ownership and state feedback

4. For a surface with competing gestures, use [mobile-gesture-interactions](../mobile-gesture-interactions/SKILL.md) to define precedence and cancellation. Keep explicit accessible controls for essential actions; a child activation must not also dispatch a parent gesture.
5. Give each applicable state a truthful message and next step: loading or progress while work is pending; a contextual empty state with a reachable valid action; errors describing what failed and whether retry is safe; offline state distinguishing retained local work from unsent work; and conflicts identifying the newer state and choices without silently discarding edits. Cancellation is not failure. Do not show success until the operation actually succeeds, or conflate local completion with remote completion. Permission or read-only limits should be visible before offering an action the user cannot perform. Avoid blocking unrelated local tasks for optional network failures.
6. Check accessible names and roles for icon-only controls, explicit status announcements, focus order, dynamic text and display scaling, contrast in supported appearances, and platform-recommended minimum touch target sizes including the full tappable bounds. Convey state through text or semantics as well as color or icon; ensure clipped labels, badges, and long user-generated strings do not obscure controls. Keep gesture alternatives operable by screen reader and keyboard/switch access where supported.

## Prove the UI, not merely the layout code

7. Run the actual app on each supported platform using a phone or simulator. Capture screenshots of the primary task and its applicable empty, loading, failure/offline, and conflict/read-only states; also capture a compact or resized window, enlarged text, and each supported appearance where those change the UI. Inspect safe edges, overlapping controls, keyboard occlusion, truncation, focus visibility, and whether state copy matches the real outcome. Use safe test data and redact private information before sharing evidence.
8. Alongside screenshots, record an interaction trace: starting state, gesture or accessible control used, resulting screen/status and underlying effect. Exercise back/cancel with and without edits, scrolling versus taps, drag versus pinch or selection where applicable, interrupted operations, permission denial, reconnect/retry if relevant, and screen-reader traversal/activation. Verify intended actions happen once, competing actions do not fire, reachable recovery works, and task/position survive rotation or resizing. Screenshots establish visual state; interaction evidence establishes behavior. Note the device/simulator, OS, viewport/configuration, and any untested platform or state rather than claiming visual or accessibility proof from source inspection alone.
