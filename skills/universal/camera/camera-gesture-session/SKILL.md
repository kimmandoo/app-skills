---
name: camera-gesture-session
description: Use when mapping tap, double-tap, and hold gestures to camera actions without corrupting an asynchronous capture session.
---

# Camera gesture and session policy

## Establish the boundary

Keep gesture disambiguation at the input boundary (native UI, Flutter widget, or another UI) and route actions and camera callbacks through one session state machine for the camera feature. For a multi-platform app, equivalent implementations may use the same transition contract; no shared code or second platform is required. The policy consumes `Tap(x, y)`, `DoubleTap`, `LongPressStart`, and `LongPressEnd`; the camera integration reports open, recording, media-save, lens, and failure events. Normalize touch coordinates to `[0, 1]` in top-left preview-view space before mapping to the sensor. Provide visible, accessible controls and a clear indication of camera use and recording.

## Implement the transitions

1. Define idle/opening/ready/start-pending/recording/stop-pending/closing and failure transitions appropriate to the camera API's *asynchronous* callbacks. Do not treat a video-start request returning as a recording-start event or a stop request as a successfully saved file. Serialize events through one session event queue/executor before updating state.
2. Only accept photo and lens-switch requests while ready. A tap requests metering at the mapped point and then a photo **after** metering is ready (see [camera-focus-orientation](../camera-focus-orientation/SKILL.md)). Prevent a second photo, lens reconfiguration, or competing start while the first operation is in flight if the camera API cannot safely handle it.
3. A hold requests video start; release, cancellation, loss of foreground, or close requests exactly one stop when start is pending or recording. Finalization/saving may complete after the stop request: keep ownership of the recording and report success only on its completion callback. Forbid lens switching while recording or finalizing.
4. Disambiguate gestures before dispatching actions. Delay the single tap until the double-tap window expires, cancel it if a second tap or long press wins, and emit one matched hold-start/hold-end pair. A double tap must not also take a photo. Reject or clamp out-of-bounds coordinates after accounting for view inset, orientation, and touch cancellation.
5. Provide clear visual and/or audible indication during camera use and recording, plus accessible status and error feedback. Haptics may supplement, never replace, required indications or consent. Do not report success on a request, capture start, or stop callback when final media saving can still fail.

## Checks

On each target platform/device, perform single tap, rapid double tap, hold/release, hold interrupted by backgrounding, lens switch during pending/active recording if supported, denied camera/media permission, session interruption, and a failed save. Verify one output per intended photo/video, no photo on a double tap, finalized or explicitly failed media after lifecycle stop, no lens reconfiguration during recording, explicit consent before capture, and clear indication until capture actually ends. Also exercise late or out-of-order start/stop/save callbacks; a stale callback must not reopen or reassign another session. Test transition invariants in the session policy, then verify real gestures, indicators, and saved files on device.
