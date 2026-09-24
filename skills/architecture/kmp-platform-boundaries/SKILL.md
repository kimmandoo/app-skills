---
name: kmp-platform-boundaries
description: Use when designing or changing a Kotlin Multiplatform module consumed by native Android and iOS apps, especially when deciding what logic to share, exporting an iOS framework, bridging asynchronous APIs, or managing native resource lifetimes.
---

# KMP platform boundaries

Share a contract only when both platforms need the same **meaning**, not merely because they perform a similar operation. Keep native UI and OS-owned resources where their lifecycles, permissions, rendering, and accessibility can be handled correctly.

## Decide the boundary

1. Map the user action end to end on Android and iOS. Identify which rules must agree (domain state, validation, transformations, ordering, operation policy) and which depend on OS APIs (UI gestures and widgets, pickers, permissions, file access, media/graphics engines, system sheets). Put portable rules and small immutable values in `commonMain`; keep Compose and SwiftUI views in their native apps. An Android-only concern need not acquire a common abstraction.
2. Specify the crossing in both directions: input values, result/error variants, coordinate or unit conventions, ownership, callback thread, and disposal. Translate native results into shared value types at the adapter boundary. Pass opaque, scoped handles for large resources; do not pass raw pixel arrays or platform image/file/URI objects through common APIs or reconstruct native paths in shared code.
3. Keep transient UI state separate from durable/shared domain state. The native UI owns focus, navigation, gestures, and accessibility; shared logic may own the policy for an already-disambiguated action and its state transitions. If a platform operation is needed, inject a narrow port with a native implementation rather than moving the OS SDK into shared code.

## Organize source sets and Swift consumption

1. Inspect the existing Gradle target and source-set graph before editing. Place common code and platform-independent tests in `commonMain`/`commonTest`; place Android or iOS implementations in the respective platform source sets **only when the module owns them**. Otherwise wire the adapters in the native application. Avoid a shared UI module unless a shared rendering strategy is an explicit product decision.
2. Configure the actual iOS device and simulator targets needed by the app, and produce a named Kotlin/Native framework imported by Swift. Check the app's framework linkage or embedding and architecture settings against those targets. Choose static or dynamic linkage to fit the host integration rather than copying an example. If a shared dependency's types appear in the public framework API, explicitly export that dependency as appropriate and confirm those types are visible to Swift; keep implementation-only dependencies unexported.
3. Design the **exported** API from the generated Swift view, not only Kotlin call sites. Prefer concrete request/result/error types, simple immutable values, explicit operations, and a subscription token with `dispose`/`close` when state observation is needed. Avoid assuming Kotlin `Result`, nested generics, coroutine `Flow`, sealed hierarchies, or `suspend` signatures yield pleasant, stable Swift interfaces. Inspect the generated framework and compile a small Swift consumer after changing the boundary.

## Preserve asynchronous and resource ownership

1. Give each operation a defined owner, completion rule, cancellation path, and release point. For callback-based native APIs, a Swift-friendly `start(request, completion) -> cancellable operation` can be wrapped in Kotlin cancellation where needed. Completion must settle at most once; cancellation must be idempotent. Specify how callbacks arriving off the UI thread are marshaled before state changes or native UI updates.
2. Distinguish logical cancellation from the underlying task actually stopping. Reject stale callbacks using an operation/session identity (and a revision when the state snapshot can change); release their returned handles even if their results are ignored. Retain input leases until an uncancellable native task finishes. Release a replaced preview only after the UI has switched away; retain a shared output until its external consumer no longer needs it. Closing a screen/session cancels work, removes observers, and releases owned resources without invalidating another active consumer.
3. Do not store heavyweight native resources in undo history or shared snapshots. Serialize shared state transitions; run expensive native work outside the UI thread. Keep visible progress, failure, cancellation, and retry states distinct where the action requires them, and never report an outdated completion as the current result.

## Verify the real boundary

- In `commonTest`, exercise value invariants and policy transitions: invalid inputs, operation ordering, cancellation/stale-result rejection, state restoration, and error distinctions. Use small fake ports to check meaningful outcomes, not that a delegate method was called. Test the native adapters' coordinate/unit conversion and resource cleanup in platform-specific tests where practical.
- Build and import the iOS framework from Swift and compile the Android consumer, but **do not stop at a build**. On an Android emulator/device and an iOS simulator/device, use each app's actual native UI to perform the same supported scenario: acquire or create an input, start a shared-domain operation backed by the native adapter, observe the resulting state, and consume its output. Compare semantic results rather than demanding pixel-identical platform rendering.
- On both platforms, cancel or leave the screen mid-operation, then start a different operation. Confirm a late callback cannot replace the new result, both screens remain responsive, observers detach, and native handles are released after their last consumer finishes. Exercise permission denial or adapter failure through the real surface when that operation uses permissions. Record which checks ran on which devices; a framework build, install, or common unit test does not prove native integration.
