---
name: camera-media-output
description: Use when building a camera that captures still frames or video to the platform media library while controlling preview, audio, permissions, and save/finalization failures.
---

# Camera media output

## Choose the actual capture contract

Determine still resolution/quality, preview policy, video audio policy, storage destination, and local legal/accessibility requirements **before** choosing capture APIs. Gengar is an experimental no-preview, no-audio camera: Android binds CameraX `ImageCapture` and `VideoCapture` without `Preview`; iOS takes stills from `AVCaptureVideoDataOutput` and records video with `AVCaptureMovieFileOutput` without an audio input. These are platform examples, not proof that every device or jurisdiction permits or guarantees inaudible capture. Avoid device-specific shutter-sound guarantees; verify on the actual hardware and do not bypass local requirements.

## Capture and publish

1. Request only necessary camera and media-add permissions. Do not request microphone access if video must be silent, and do not connect an audio source to the recording pipeline. Permission denial or interruption must produce an observable error, not an apparent no-op.
2. Configure a supported camera session and an explicit still/video resolution policy. With an iOS frame-grab pipeline, use a frame from the video data output; document that available still resolution and features may differ from dedicated photo capture. Own the sample buffer until encoding completes, encode once, and apply the chosen still orientation exactly once (see [camera-focus-orientation](../camera-focus-orientation/SKILL.md)).
3. On Android, save photo/video through the platform's MediaStore output options using correct MIME type and collection; update target rotation when there is no preview use case. On iOS, write encoded still data or a finalized video file using Photos' add-only flow if the product uses the system library. Keep a temporary video file until the Photos completion callback and remove it on both success and failure after cleanup is safe.
4. Surface separate events for request accepted, recording started, recording stopped, video finalized, and library save succeeded/failed. Do not equate a stopped recording with durable media. When save or encoding fails, clean up partial output according to platform rules and avoid reporting a phantom successful photo/video. Use collision-resistant media filenames or IDs; timestamps rounded to seconds alone may collide under rapid capture.
5. Handle lifecycle stop, camera interruption, and permission revocation by finalizing or explicitly failing in-flight work. Keep screen-awake state only while the active camera requires it, and restore the prior setting on exit.

## Verification

On physical Android and iOS devices, inspect library entries and decoded files after rapid photos, front/back camera, recording release, backgrounding mid-recording, low storage, permission denial, and camera interruption. Confirm photo and video media types, orientation, resolution, playable video, actual absence of an audio track if required, and no empty/duplicate or partial entries after failure. Listen on the target devices/regions if an audible-output requirement exists; source-token checks alone do not establish acoustic behavior or compliance. Record build, device/OS, observed sound, and saved-media evidence. No camera available in a simulator means compilation-only evidence.

Source: Gengar `README.md`, `docs/DESIGN_RULES.md`, Android `CameraXSilentCamera.kt`, iOS `SilentCameraController.swift`, and `SilentCaptureContractTest.kt` (a source-token guard, not a device-level sound test).
