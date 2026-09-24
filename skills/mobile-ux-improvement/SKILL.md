---
name: mobile-ux-improvement
description: Use when improving an existing mobile task flow or perceived responsiveness, prioritizing friction, wayfinding, truthful feedback, and restrained motion over decorative redesign.
---

# Improve one mobile task at a time

This is an **improvement** workflow: diagnose a real task, make a small change, and compare the experience before and after. For comprehensive screen-state and accessibility coverage, use [mobile-ui-ux-verification](../mobile-ui-ux-verification/SKILL.md); for conflicting touch input, use [mobile-gesture-interactions](../mobile-gesture-interactions/SKILL.md). Do not invent features, a new visual language, or performance claims to justify polish.

## Find the expensive moment

1. Identify the user's goal and trace the current path on the running app: entry, discovery, action, immediate response, completion, cancellation, and recovery. Include permission, offline, or destructive paths **only if the product has them**. Capture the screen and interaction trace before editing; record the actual device, build, text size, and network state. If the app cannot run, mark the baseline as unobserved instead of treating code inspection as a UX result.
2. Locate the interface implementation, existing design tokens/components, platform navigation conventions, and the state transition that produces each visible message. Use support reports or observed task failures when available; otherwise label likely friction as a hypothesis. Check for hidden primary actions, misleading success, repeated confirmations, inaccessible controls, late press feedback, and transitions that delay input or discard context.
3. Rank candidates by user impact, task frequency, risk of an error, and implementation cost. Choose the smallest change that removes a consequential obstacle. Record at least one tempting change you rejected (for example, motion on a frequently used control) and why. A preference for more animation is not evidence of better UX.

## Make the path clearer before adding motion

4. Put the next valid action where people expect it; use specific labels, a visible way out, and clear ownership of destructive actions. Keep native navigation and the product's established typography, color, spacing, and assets. Reveal advanced options without hiding essential context. An empty screen should explain what to do next; an error should distinguish retryable from irreversible failure. If work is saved locally but not remotely, name those states separately. Never show completion before the underlying operation succeeds.
5. If a change needs motion, name its **purpose**: feedback, spatial continuity, state recognition, or preventing a jarring swap. Gauge how often it will occur. For high-frequency paths, prefer platform defaults or no extra animation; reserve conspicuous movement for infrequent, useful moments. Do not delay input, navigation, or readable content for delight. Choose the existing platform animation mechanism and design tokens rather than transplanting web CSS or Expo-specific APIs into a native or Flutter app.
6. For the remaining motion, give press feedback promptly, preserve the spatial relationship between a trigger and the resulting surface, and dismiss along a comprehensible return path. A gesture-driven element should follow the finger and retarget from its current displayed position when interrupted; it must not jump, lock input, or restart from its original position. Prefer compositor-friendly properties when feasible, but measure on the target framework: blanket bans on layout changes are not a substitute for profiling. Use haptics sparingly at a meaningful committed event, paired with a visible state; never use them as the sole confirmation. Respect the system's reduced-motion setting without removing essential state feedback.
7. Implement the selected improvement end to end, including its affected states and accessible control path. Keep state authority in the existing domain layer; do not use an animation timer to decide whether an operation succeeded. Update existing behavioral contracts only when the intended product behavior changes.

## Compare the outcome

8. Replay the same task on the changed build using comparable device/configuration and data. Observe press-to-feedback timing, number of steps, status accuracy, and whether a user can finish or undo the action. Exercise a rapid repeat, cancellation or reversal during motion, large text, reduced motion, screen reader or switch/keyboard access where supported, and a failed operation. Include offline or permission denial only if the flow uses them. Check dropped frames or main-thread stalls on a representative slower device when motion changed; do not claim smoothness from source code or a simulator alone.
9. Report a short **Before | After | Why | Evidence** table for the changes made, plus rejected candidates and any unverified platform/device. A before/after screenshot shows appearance, not task completion: pair it with an interaction trace and actual outcome. If the change did not improve the task or made repeated use slower, revert or simplify it. Do not invent measured time savings or user-test results.

Adapted for native/Kotlin Multiplatform and Flutter mobile workflows from the motion-restraint and audit principles in [Emil Kowalski's skills](https://github.com/emilkowalski/skills) (MIT; [upstream license](UPSTREAM_LICENSE)). The source's CSS, React Native, and Expo recipes are **not** assumed to apply to the consuming app.
