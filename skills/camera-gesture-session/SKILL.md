---
name: camera-gesture-session
description: Use when mapping tap, double-tap, and hold gestures to camera actions across platform-native UIs without corrupting an asynchronous capture session.
---

# Camera gesture and session policy

## Establish the boundary

Keep platform touch disambiguation in the native UI and action policy in one state machine when Android and iOS must behave alike. Gengar's `sharedLogic/SilentCameraSession` is an example: it consumes `Tap(x, y)`, `DoubleTap`, `LongPressStart`, `LongPressEnd`; camera adapters emit `Opened`, `Closed`, recording, saved-media, lens, and failure events. Coordinates are normalized to `[0, 1]` in top-left screen space. Adapt the gestures and feedback to the consuming product; a black, haptic-only UI is not a general accessibility pattern.

## Implement the transitions

1. Define idle/opening/ready/start-pending/recording/stop-pending/closing and failure transitions appropriate to the adapter's *asynchronous* callbacks. Do not treat `startVideo()` returning as a recording-start event or a stop request as a successfully saved file. Serialize events on the session's chosen executor before updating state.
2. Only accept photo and lens-switch requests while ready. A tap requests metering at the mapped point and then a photo **after** metering is ready (see [camera-focus-orientation](../camera-focus-orientation/SKILL.md)). Prevent a second photo, lens rebind, or competing start while the first operation is in flight if the adapter cannot safely handle it.
3. A hold requests video start; release, cancellation, loss of foreground, or close requests exactly one stop when start is pending or recording. Finalization/saving may complete after the stop request: keep ownership of the recording and report success only on its completion callback. Forbid lens switching while recording or finalizing.
4. Disambiguate gestures before dispatching actions. Delay the single tap until the double-tap window expires, cancel it if a second tap or long press wins, and emit one matched hold-start/hold-end pair. A double tap must not also take a photo. Reject or clamp out-of-bounds coordinates after accounting for view inset, orientation, and touch cancellation.
5. Map save success and errors to distinct feedback. If the UI has no visible status, provide an accessible alternative for permission denial, failure, and recording state; haptics alone are not sufficient for all users. Do not report success on a request, capture start, or stop callback when final media saving can still fail.

## Checks

On each platform, perform single tap, rapid double tap, hold/release, hold interrupted by backgrounding, lens switch during pending/active recording, denied camera/storage permission, session interruption, and a failed save. Verify one output per intended photo/video, no photo on a double tap, finalized media after lifecycle stop, no lens rebind during recording, and distinguishable failure feedback. Also exercise late or out-of-order start/stop/save callbacks; a stale callback must not reopen or reassign another session. A shared-policy unit test checks transition invariants, but native-device gestures, haptics, and saved files require device verification.

Source: Gengar `docs/DESIGN_RULES.md` (gesture and state contract), `sharedLogic/SilentCamera.kt` (event policy), native gesture UIs and `SilentCameraSessionTest.kt`. The pending states and delayed tap are robustness requirements for consumers, not claims that the source already implements them.
